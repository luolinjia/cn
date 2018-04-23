<!-- <section class="comment">
<div id="disqus_thread"></div>
<script type="text/javascript">
    /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
    var disqus_shortname = 'ideex'; // required: replace example with your forum shortname
    var disqus_url = '{{ site.url }}{{ page.url | remove:'index.html' }}';
    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {
        var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
        dsq.src = 'https://' + disqus_shortname + '.disqus.com/embed.js';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
</section> -->

<!-- use Valine comments system -->
<!-- https://valine.js.org/quickstart -->
<div id="comment"></div>
<script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
<script src='//unpkg.com/valine/dist/Valine.min.js'></script>
<script>
new Valine({
    el: '#comment',
    notify: false, 
    verify: false, 
    appId: 'ypxYhS2Dey2BTRKlzAxbpeuD-gzGzoHsz',
    appKey: 'o0S8rLNQlX7MBjnVz3BvafkG',
    placeholder: '不想说点什么吗？',
    path: window.location.pathname, 
    avatar: 'mm' 
});
</script>