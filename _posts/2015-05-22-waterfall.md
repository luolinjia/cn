---
layout: post
title: 图片瀑布流
categories:
- Work
tags:
- 瀑布流
- Javascript
- jQuery
---

> 图片瀑布流插件  

## 图片瀑布流原理  

其实整个图片瀑布流的实现原理比较简单，只要了解了整个流程即可。

![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/0522-2015_zpsvmuiw8az.png)  

根据上图，能够看出一个基本的瀑布流原理： 先放好第一排的图片，然后计算第一排图片的innerHeight，也就是高度值，找出高度值最小的，把下一张图片（这里是图片4）放到高度最小值的下面，后面的图片循环依此类推。

- 一般来讲，瀑布流都是定宽
- 列数可固定，可自适应（本例中为最开始固定列数）
- 图片大小等比例缩放
- 下拉ajax异步请求数据  

基本上都是基于用absolute来定位图片的位置摆放。  


## 一种新的解决思路  

![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/0522-2015-2_zpsefmuleqd.png)  

首先，根据插件可以预知具体的列数，那么就生成对应的几个容器DIV（float: left;），这样的好处：  

- 无需计算高度（但ajax获取的数据图片信息里面要包含图片的高宽）
- 由于直接是DIV放DIV容器，布局默认的就可以，不用像上面一样计算复杂的absolute布局的div
- 提高加载性能  

使用方法：  
{% highlight javascript %}
// waterfall
$('#textBox').waterfall({
	col: 3, 
	width: 240,
	jsonURL: 'data/data.json',
	onAfterLoad: function (dom) {
		// TODO
	}
});
{% endhighlight %}

附插件代码（基于jQuery）：  
{% highlight javascript %}
/**
 * Waterfall
 * @auth: Karl Luo(360512239@qq.com)
 * @dependence jQuery
 * @createDate 04-19-2015
 * @usage:
 * <div id="testBox">
 *
 * </div>
 *
 * $('#testBox').waterfall({
 *      col: 3, 						// set the display column number
 *      width: 240,  					// set the each column color
 *      jsonURL: 'data/data.json',   	// provide a json URL for ajax
 *      //limit: 0,  					// the reserved parameter
 *      onAfterLoad: function(param){}	// add the callback for the every img rendering dom
 * });
 **/
(function($){
	$.fn.waterfall = function (options) {
		var flag = true,
			o = $(this),
			settings = {
				col: 3,
				width: 240,
				jsonURL: 'data/data.json',
				limit: 0, // -1 | 0 | 2000
				onAfterLoad: function (param) {}
			}, req = {
				/**
				 * request the json data from server
				 * @param {Object} options
				 * @param {Function} callback
				 */
				reqJsonData: function (options, callback) {
					$.ajax($.extend({
						type: 'POST',
						dataType: 'JSON'
					}, options, true)).done(function(data){
						if (data && $.isFunction(callback)) {
							callback(data.sort(_.jsObjQueue('timestamp')));
						}
					});
				}
			}, _ = {
				/**
				 * render the 3 column divs
				 */
				renderBox: function () {
					var divs = [], i = 0, size = settings.col;
					for (; i < size; i++) {
						divs.push('<div class="wf-box"></div>');
					}
					o.append(divs.join('')).append('<div id="loading">正在加载……</div>');
				},
				/**
				 * render the popup layer
				 * @param {String} data
				 */
				popupLayer: function (data) {
					var dom = '<div id="popup_div" class="popup-div"><div class="popup-div-content"><img src="' + data + '" alt=""/></div></div><div id="bg"></div>';
					$('body').append(dom);
					_.bindCloseEvent();
				},
				/**
				 * bind Close Event for popup layer
				 */
				bindCloseEvent: function () {
					$(document).click(function (e) {
						var pop = $('#popup_div');
						if (!pop.is(e.target) && pop.has(e.target).length === 0) {
							$('#bg, #popup_div').remove();
						}
					});
				},
				/**
				 * set the default position about the new img div
				 * @param {Object} data
				 */
				setDefaultPos: function (data) {
					var i = 0, size = data.length, colNo = settings.col, wfBoxes = $('.wf-box', o);
					for (; i < size; i++) {
						var item = data[i], dom = '<div class="mb20"><div class="wf-box-title"><a href="javascript:;"><img data-url="' + item['img']['img'] + '" src="' + item['img']['img'] + '" alt=""/></a><p><span>' + item['title'] + '</span></p></div><div class="wf-box-info"><span class="wf-box-info-price"><span>￥' + item['price'].toFixed(2) + '</span>元</span><span class="wf-box-info-sale">已售出<span>' + item['sale'] + '</span>次</span></div></div>';
						if (i < colNo) {
							$(wfBoxes[i]).append($(dom).fadeIn());
							$.isFunction(settings.onAfterLoad) && settings.onAfterLoad($(dom));
						} else {
							var minH = $(wfBoxes[0]).outerHeight(), j = 0, bSize = wfBoxes.length;
							for (; j < bSize; j++) {
								var wfH = $(wfBoxes[j]).outerHeight();
								if (wfH < minH) {
									minH = wfH;
								}
							}
							var key = _.getKey(wfBoxes, minH);
							$(wfBoxes[key]).append($(dom).fadeIn());
							$.isFunction(settings.onAfterLoad) && settings.onAfterLoad($(dom));
						}
					}
					$('img', o).css({'width': settings.width - 20}).click(function (e) {
						e.stopPropagation();
						var thiz = $(this);
						_.popupLayer(thiz.attr('data-url'));
					});
					$('p', o).css({'width': settings.width - 20});
				},
				/**
				 * determine the relevant key
				 * @param {Array} a
				 * @param {String} v
				 * @returns {*}
				 */
				getKey: function (a, v) {
					var i = 0, size = a.length, key;
					for (; i < size; i++) {
						if ($(a[i]).outerHeight() === v) {
							key = i;
						}
					}
					return key;
				},
				/**
				 * scroll down the scrollBar, and get More images
				 */
				loadMore: function () {
					$("#loading").fadeIn('fast');
					req.reqJsonData({url: settings.jsonURL}, function (data) {
						_.setDefaultPos(data);
						$('#loading').fadeOut('fast');
						flag = true;
					});
				},
				/**
				 * bind the scroll event with window Object
				 */
				scrollEvent: function () {
					$(window).scroll(function(){
						if (flag && ($(document).height() - $(this).scrollTop() - $(this).height() < 100)) {
							flag = false;
							_.loadMore();
						}
					});

				},
				/**
				 * get the queue about the sort result
				 * @param key
				 * @returns {Function}
				 */
				jsObjQueue: function (key) {
					return function (o, p) {
						var x, y;
						if (o && p && typeof o === 'object' && typeof p === 'object') {
							x = o[key];
							y = p[key];
							if (x === y) {
								return 0;
							}
							if (typeof x === typeof y) {
								return x < y ? 1 : -1;
							}
							return typeof x < typeof y ? 1 : -1;
						} else {
							return 1;
						}
					}
				}
		};

		settings = $.extend(settings, options);
		req.reqJsonData({url: settings.jsonURL}, function (data) {
			_.renderBox();
			_.setDefaultPos(data);
			_.scrollEvent();
		});
	};
})(jQuery);
{% endhighlight %}  

附插件CSS代码：  
{% highlight css %}
#bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
}
#popup_div {
    position: fixed;
    z-index: 9999;
    opacity: 1;
    width: 70%;
    height: 75%;
    left: 15%;
    top: 10%;
    background-color: #fff;
    color: #000;
    border: 5px solid #ccc;
}
#popup_div .popup-div-content {
    text-align: center;
}
{% endhighlight %} 