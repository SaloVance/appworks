/**
 * page scroll for appworks
 * https://github.com/SaloVance/appworks
 *
 * @author: dingguolong@baidu.com
 * @date: 2014-05-20
 * @version: 1.1
 */
$(function(){
  var supports = function(prop) {
    var div = document.createElement('div'),
        vendors = '-khtml- -ms- -o- -moz- -webkit-'.split(' '),
        len = vendors.length;
    if ( prop in div.style ){
      return prop;
    }
    while(len--) {
      var p = vendors[len] + prop;
      if ( p in div.style ) {
        return p;
      }
    }
    return '';
  };
  /* page scroll */
  var pageScroll = {
    init: function($container, $slides, $trigger, $backTop, $indicator) {
      var self = this;
      this.current = 0; /* index of slide, from 0 */
      this.$el = $container;
      this.$slides = $slides;
      this.$trigger = $trigger;
      this.$backTop = $backTop;
      this.$indicator = $indicator;
      this.isScroll = false;
      /* this.transform = supports('transform');  */
      this.transform = '';
      setTimeout(function(){$($slides[0]).addClass('current')}, 300);
      this.transform && this.$el.addClass('transform'); 
      
      var wheelEvent = (function(){
        var isFirefox = /firefox/i.test(navigator.userAgent),
            mousewheel = isFirefox ? 'DOMMouseScroll' : 'mousewheel';
        return {
          dispath: function(dom, callback) {
            if (document.addEventListener) {
              dom.addEventListener(mousewheel, callback, false);
            } else {
              dom.attachEvent('on' + mousewheel, callback);
            }
          },
          getWheelDelta: function(e) {
            return isFirefox ? Math.abs(e.detail) / e.detail * -1 : Math.abs(e.wheelDelta) / e.wheelDelta;
          }
        };
      })();
  
      /* mouse event */
      wheelEvent.dispath(document, function(e) {
        var dir = wheelEvent.getWheelDelta(e);
        var scroll = (dir == 1) ? 'scrollUp' : 'scrollDown' ;
        self[scroll]();
      });
      
      /* keyboard event */
      $(document).on('keyup', function(e) {
          if (e.keyCode == 33 || e.keyCode == 38) {
              pageScroll.scrollUp(); /* arrow up  */
          } else if (e.keyCode == 32 || e.keyCode == 34 || e.keyCode == 40) {
              pageScroll.scrollDown(); /* arrow down */
          }
      });
    },
    lock: function(){
      this.isScroll = true;
      this.$trigger.fadeOut();
    },
    unlock: function(){
      var current = this.current;
      /* $(this.$slides[current]).addClass('current').siblings().removeClass('current'); */ 
      if(current == (this.$slides.length-1)){
         this.$trigger.hide();
      }else{
         this.$trigger.fadeIn();
      }
      this.isScroll = false;
    },
    
    scrollUp: function(){
      if( this.isScroll || this.current == 0){
        return;
      }
      this.scrollTo(this.current - 1);
    },
    
    scrollDown: function(){
      if( this.isScroll || this.current == (this.$slides.length-1)){
        return;
      }
      this.scrollTo(this.current + 1);
    },
    
    scrollTo: function(page){
      var self = this;
      if((page < 0) || (page >(this.$slides.length-1))){
        return;
      }
      this.lock();
      this.current = page;
      
      if(this.current != 0){
        this.$backTop.fadeIn();
      }else{
        this.$backTop.fadeOut();
      }
      
      $(this.$slides[this.current]).addClass('current').siblings().removeClass('current'); 
      this.$indicator.find('li').eq(this.current).addClass('current').siblings().removeClass('current'); 
      
      var winHeight = $(window).height();
      var top = winHeight * this.current;
      var trans = this.transform;
      if(trans){
        this.$el.css(trans, 'translateY(-'+ top +'px)');
        setTimeout(function(){self.unlock()}, 800);
      }else{
        this.$el.animate({top: '-'+ top +'px'}, 800, 'swing',
          function(){
            self.unlock();
        });
      }
    }
  };
  
  var $container = $('.container'),
      $slides = $('.section'),
      $trigger = $('.home-mouse'), /* trigger scroll down */
      $backTop = $('.back-top'),  /* trigger scroll up */
      $indicator = $('.indicator'); /* indicate current page */
      
  /* mobile event */
  $container.swipeUp(function() {
    pageScroll.scrollUp();
  }).swipeDown(function() {
    pageScroll.scrollDown();
  });
  
  var desktop = !navigator.userAgent.match(/(iPhone|iPod|iPad|Android|BlackBerry|BB10|mobi|tablet|opera mini|nexus 7)/i),
    clickEventName = desktop ? 'click' : 'touchstart'; /* pc & mobile click */
  $trigger.on(clickEventName, function() {
    pageScroll.scrollDown();
  });
  $backTop.on(clickEventName, function() {
    pageScroll.scrollUp();
  });
  $('li', $indicator).on(clickEventName, function() {
    var index = $(this).data('index');
    pageScroll.scrollTo(+index);
  });
  
  var first = true;
  function resize(){
    var winWidth = $(window).width(),
        winHeight = $(window).height();
        
    (winWidth<800) && (winWidth = 800);
    (winHeight<500) && (winHeight = 500);
    $slides.each(function(i){
      $(this).css({width: winWidth+'px', height: winHeight+'px', top: i*winHeight, display: 'block'});
    });
    $container.height(winHeight*$slides.length);
    
    if(first){
      pageScroll.init($container, $slides, $trigger, $backTop, $indicator);
      first = false;
    }else{
      pageScroll.scrollTo(0);
    }
  }
  resize();
  $(window).on('resize', resize);
  
  function isIE(){
    var browser=navigator.appName;
    var b_version=navigator.appVersion;
    var version=b_version.split(";");
    var trim_Version= version[1] && version[1].replace(/[ ]/g,"");
    return (browser=="Microsoft Internet Explorer") && (trim_Version=="MSIE6.0" || trim_Version == 'MSIE7.0' || trim_Version == 'MSIE8.0' || trim_Version == 'MSIE9.0');
  }
  if(!!isIE()){
    var $panel = $('#goodbyeIE');
    $panel.css({
      top: $(window).height() - $panel.height()
    }).fadeIn(1000);
    $('#closeBrowserTip').click(function() {
      $panel.fadeOut(1000);
    });
  }
});
