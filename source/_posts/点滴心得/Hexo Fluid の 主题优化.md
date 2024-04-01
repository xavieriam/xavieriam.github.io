---
title: Hexo Fluid の 主题优化
index_img: /img/index_img/blog.png
categories:
  - 点滴心得
date: 2024-03-27 00:00:00
archive:
category_bar: true
---
基础配置参照官方指南
{% note info %}
[配置指南 | Hexo Fluid 用户手册 (fluid-dev.com)](https://hexo.fluid-dev.com/docs/guide/#%E5%85%B3%E4%BA%8E%E6%8C%87%E5%8D%97)
{% endnote %}

## 站点配置  `_config.yml`

### 添加网站运行时间

页脚添加网站运行时间，只需要在主题配置中的 `footer: content` 添加：
```yml
footer:
  content: '
    <div>
      <span id="timeDate">载入天数...</span>
      <span id="times">载入时分秒...</span>
      <script src="/js/duration.js"></script>
    </div>
  '
```

在主题目录下创建 `source/js/duration.js`，内容如下：
```js
var now = new Date();
function createtime() {
    var grt= new Date("03/03/2021 12:00:00");//在此处修改你的建站时间，格式：月/日/年 时:分:秒
    now.setTime(now.getTime()+250);
    days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days);
    hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours);
    if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum);
    mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;}
    seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
    snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;}
    document.getElementById("timeDate").innerHTML = "本站已安全运行 "+dnum+" 天 ";
    document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒";
}
setInterval("createtime()",250);
```

将建站时间修改为你自己的建站时间，即可在页脚加入网站运行时间。

### 主题背景美化

{% note success %}

**背景的配置方法**

1. 在主题目录下创建 JavaScript 文件 `themes/fluid/source/js/example.js` 

2. 在主题配置 `_config.yml` 中找到 `custom_js` 或者 `custom_css`，进行添加修改：

```yml
custom_js:
  - /js/DynamicLine.js
  - /js/DynamicRibbon.js # 动态彩带
  - /js/runtime.js # 运行时间
  # - /js/Ribbon.js # 静态彩带
  # - /js/DynamicLine.js # 动态黑色线条
  # - /js/snowflake.js # 小雪花飘落
  # - /js/Cherry.js #樱花飘落
  # - /js/star.js # 鼠标跟随小星星
  # - /js/containsWord.js # 鼠标点击出字
  # - /js/love.js #鼠标点击出爱心

custom_css:
  - /js/shubiao.css #鼠标指针
  - /js/scroll.css # 滚动条颜色
  # - /js/gradient.css # 头部打字机颜色效果渐变
```

{% endnote %}

{% note primary %}

**详细背景样式参考**

{% endnote %}

{% fold info @动态彩带 DynamicRibbon.js %}

```js
//动态彩带
(function (name, factory) {
    if (typeof window === "object") {
        window[name] = factory()
    }
})("Ribbons", function () {
    var _w = window,
        _b = document.body,
        _d = document.documentElement;
    var random = function () {
        if (arguments.length === 1) {
            if (Array.isArray(arguments[0])) {
                var index = Math.round(random(0, arguments[0].length - 1));
                return arguments[0][index]
            }
            return random(0, arguments[0])
        } else if (arguments.length === 2) {
            return Math.random() * (arguments[1] - arguments[0]) + arguments[0]
        }
        return 0
    };
    var screenInfo = function (e) {
        var width = Math.max(0, _w.innerWidth || _d.clientWidth || _b.clientWidth || 0),
            height = Math.max(0, _w.innerHeight || _d.clientHeight || _b.clientHeight || 0),
            scrollx = Math.max(0, _w.pageXOffset || _d.scrollLeft || _b.scrollLeft || 0) - (_d.clientLeft || 0),
            scrolly = Math.max(0, _w.pageYOffset || _d.scrollTop || _b.scrollTop || 0) - (_d.clientTop || 0);
        return {
            width: width,
            height: height,
            ratio: width / height,
            centerx: width / 2,
            centery: height / 2,
            scrollx: scrollx,
            scrolly: scrolly
        }
    };
    var mouseInfo = function (e) {
        var screen = screenInfo(e),
            mousex = e ? Math.max(0, e.pageX || e.clientX || 0) : 0,
            mousey = e ? Math.max(0, e.pageY || e.clientY || 0) : 0;
        return {
            mousex: mousex,
            mousey: mousey,
            centerx: mousex - screen.width / 2,
            centery: mousey - screen.height / 2
        }
    };
    var Point = function (x, y) {
        this.x = 0;
        this.y = 0;
        this.set(x, y)
    };
    Point.prototype = {
        constructor: Point,
        set: function (x, y) {
            this.x = x || 0;
            this.y = y || 0
        },
        copy: function (point) {
            this.x = point.x || 0;
            this.y = point.y || 0;
            return this
        },
        multiply: function (x, y) {
            this.x *= x || 1;
            this.y *= y || 1;
            return this
        },
        divide: function (x, y) {
            this.x /= x || 1;
            this.y /= y || 1;
            return this
        },
        add: function (x, y) {
            this.x += x || 0;
            this.y += y || 0;
            return this
        },
        subtract: function (x, y) {
            this.x -= x || 0;
            this.y -= y || 0;
            return this
        },
        clampX: function (min, max) {
            this.x = Math.max(min, Math.min(this.x, max));
            return this
        },
        clampY: function (min, max) {
            this.y = Math.max(min, Math.min(this.y, max));
            return this
        },
        flipX: function () {
            this.x *= -1;
            return this
        },
        flipY: function () {
            this.y *= -1;
            return this
        }
    };
    var Factory = function (options) {
        this._canvas = null;
        this._context = null;
        this._sto = null;
        this._width = 0;
        this._height = 0;
        this._scroll = 0;
        this._ribbons = [];
        this._options = {
            colorSaturation: "80%",
            colorBrightness: "60%",
            colorAlpha: 0.65,
            colorCycleSpeed: 6,
            verticalPosition: "center",
            horizontalSpeed: 150,
            ribbonCount: 5,
            strokeSize: 5,
            parallaxAmount: -0.5,
            animateSections: true
        };
        this._onDraw = this._onDraw.bind(this);
        this._onResize = this._onResize.bind(this);
        this._onScroll = this._onScroll.bind(this);
        this.setOptions(options);
        this.init()
    };
    Factory.prototype = {
        constructor: Factory,
        setOptions: function (options) {
            if (typeof options === "object") {
                for (var key in options) {
                    if (options.hasOwnProperty(key)) {
                        this._options[key] = options[key]
                    }
                }
            }
        },
        init: function () {
            try {
                this._canvas = document.createElement("canvas");
                this._canvas.style["display"] = "block";
                this._canvas.style["position"] = "fixed";
                this._canvas.style["margin"] = "0";
                this._canvas.style["padding"] = "0";
                this._canvas.style["border"] = "0";
                this._canvas.style["outline"] = "0";
                this._canvas.style["left"] = "0";
                this._canvas.style["top"] = "0";
                this._canvas.style["width"] = "100%";
                this._canvas.style["height"] = "100%";
                this._canvas.style["z-index"] = "-1";
                this._onResize();
                this._context = this._canvas.getContext("2d");
                this._context.clearRect(0, 0, this._width, this._height);
                this._context.globalAlpha = this._options.colorAlpha;
                window.addEventListener("resize", this._onResize);
                window.addEventListener("scroll", this._onScroll);
                document.body.appendChild(this._canvas)
            } catch (e) {
                console.warn("Canvas Context Error: " + e.toString());
                return
            }
            this._onDraw()
        },
        addRibbon: function () {
            var dir = Math.round(random(1, 9)) > 5 ? "right" : "left",
                stop = 1000,
                hide = 200,
                min = 0 - hide,
                max = this._width + hide,
                movex = 0,
                movey = 0,
                startx = dir === "right" ? min : max,
                starty = Math.round(random(0, this._height));
            if (/^(top|min)$/i.test(this._options.verticalPosition)) {
                starty = 0 + hide
            } else if (/^(middle|center)$/i.test(this._options.verticalPosition)) {
                starty = this._height / 2
            } else if (/^(bottom|max)$/i.test(this._options.verticalPosition)) {
                starty = this._height - hide
            }
            var ribbon = [],
                point1 = new Point(startx, starty),
                point2 = new Point(startx, starty),
                point3 = null,
                color = Math.round(random(0, 360)),
                delay = 0;
            while (true) {
                if (stop <= 0) break;
                stop--;
                movex = Math.round((Math.random() * 1 - 0.2) * this._options.horizontalSpeed);
                movey = Math.round((Math.random() * 1 - 0.5) * (this._height * 0.25));
                point3 = new Point();
                point3.copy(point2);
                if (dir === "right") {
                    point3.add(movex, movey);
                    if (point2.x >= max) break
                } else if (dir === "left") {
                    point3.subtract(movex, movey);
                    if (point2.x <= min) break
                }
                ribbon.push({
                    point1: new Point(point1.x, point1.y),
                    point2: new Point(point2.x, point2.y),
                    point3: point3,
                    color: color,
                    delay: delay,
                    dir: dir,
                    alpha: 0,
                    phase: 0
                });
                point1.copy(point2);
                point2.copy(point3);
                delay += 4;
                color += this._options.colorCycleSpeed
            }
            this._ribbons.push(ribbon)
        },
        _drawRibbonSection: function (section) {
            if (section) {
                if (section.phase >= 1 && section.alpha <= 0) {
                    return true
                }
                if (section.delay <= 0) {
                    section.phase += 0.02;
                    section.alpha = Math.sin(section.phase) * 1;
                    section.alpha = section.alpha <= 0 ? 0 : section.alpha;
                    section.alpha = section.alpha >= 1 ? 1 : section.alpha;
                    if (this._options.animateSections) {
                        var mod = Math.sin(1 + section.phase * Math.PI / 2) * 0.1;
                        if (section.dir === "right") {
                            section.point1.add(mod, 0);
                            section.point2.add(mod, 0);
                            section.point3.add(mod, 0)
                        } else {
                            section.point1.subtract(mod, 0);
                            section.point2.subtract(mod, 0);
                            section.point3.subtract(mod, 0)
                        }
                        section.point1.add(0, mod);
                        section.point2.add(0, mod);
                        section.point3.add(0, mod)
                    }
                } else {
                    section.delay -= 0.5
                }
                var s = this._options.colorSaturation,
                    l = this._options.colorBrightness,
                    c = "hsla(" + section.color + ", " + s + ", " + l + ", " + section.alpha + " )";
                this._context.save();
                if (this._options.parallaxAmount !== 0) {
                    this._context.translate(0, this._scroll * this._options.parallaxAmount)
                }
                this._context.beginPath();
                this._context.moveTo(section.point1.x, section.point1.y);
                this._context.lineTo(section.point2.x, section.point2.y);
                this._context.lineTo(section.point3.x, section.point3.y);
                this._context.fillStyle = c;
                this._context.fill();
                if (this._options.strokeSize > 0) {
                    this._context.lineWidth = this._options.strokeSize;
                    this._context.strokeStyle = c;
                    this._context.lineCap = "round";
                    this._context.stroke()
                }
                this._context.restore()
            }
            return false
        },
        _onDraw: function () {
            for (var i = 0, t = this._ribbons.length; i < t; ++i) {
                if (!this._ribbons[i]) {
                    this._ribbons.splice(i, 1)
                }
            }
            this._context.clearRect(0, 0, this._width, this._height);
            for (var a = 0; a < this._ribbons.length; ++a) {
                var ribbon = this._ribbons[a],
                    numSections = ribbon.length,
                    numDone = 0;
                for (var b = 0; b < numSections; ++b) {
                    if (this._drawRibbonSection(ribbon[b])) {
                        numDone++
                    }
                }
                if (numDone >= numSections) {
                    this._ribbons[a] = null
                }
            }
            if (this._ribbons.length < this._options.ribbonCount) {
                this.addRibbon()
            }
            requestAnimationFrame(this._onDraw)
        },
        _onResize: function (e) {
            var screen = screenInfo(e);
            this._width = screen.width;
            this._height = screen.height;
            if (this._canvas) {
                this._canvas.width = this._width;
                this._canvas.height = this._height;
                if (this._context) {
                    this._context.globalAlpha = this._options.colorAlpha
                }
            }
        },
        _onScroll: function (e) {
            var screen = screenInfo(e);
            this._scroll = screen.scrolly
        }
    };
    return Factory
});
new Ribbons({
    colorSaturation: "60%",
    colorBrightness: "50%",
    colorAlpha: 0.5,
    colorCycleSpeed: 5,
    verticalPosition: "random",
    horizontalSpeed: 200,
    ribbonCount: 3,
    strokeSize: 0,
    parallaxAmount: -0.2,
    animateSections: true
});
```
{% endfold %}

{% fold info @运行时间 runtime.js %}
```js
//运行时间bynote.cn
var now = new Date(); 
function createtime() { 
    var grt= new Date("03/03/2021 12:00:00");//在此处修改你的建站时间，格式：月/日/年 时:分:秒
    now.setTime(now.getTime()+250); 
    days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days); 
    hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours); 
    if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum); 
    mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;} 
    seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum); 
    snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;} 
    document.getElementById("timeDate").innerHTML = "本站已安全运行 "+dnum+" 天 "; 
    document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒"; 
} 
setInterval("createtime()",250);
```
{% endfold %}

{% fold info @静止彩色丝带 Ribbon.js %}
```js
/** 静止彩色丝带bynote.cn **/
/*jshint -W030 */
! function() {
  function attr(node, attr, default_value) {
    return Number(node.getAttribute(attr)) || default_value;
  }

  // get user config
  var scripts = document.getElementsByTagName('script'),
    script = scripts[scripts.length - 1]; // 当前加载的script
  config = {
    z: attr(script, "zIndex", -1), // z-index
    a: attr(script, "alpha", 0.6), // alpha
    s: attr(script, "size", 90), // size
  };

  var canvas = document.createElement('canvas'),
    g2d = canvas.getContext('2d'),
    pr = window.devicePixelRatio || 1,
    width = window.innerWidth,
    height = window.innerHeight,
    f = config.s,
    q, t,
    m = Math,
    r = 0,
    pi = m.PI*2,
    cos = m.cos,
    random = m.random;
  canvas.width = width * pr;
  canvas.height = height * pr;
  g2d.scale(pr, pr);
  g2d.globalAlpha = config.a;
  canvas.style.cssText = 'opacity: ' + config.a + ';position:fixed;top:0;left:0;z-index: ' + config.z + ';width:100%;height:100%;pointer-events:none;';
  // create canvas
  document.getElementsByTagName('body')[0].appendChild(canvas);

  function redraw() {
    g2d.clearRect(0, 0, width, height);
    q = [{x: 0, y: height * 0.7 + f}, {x: 0, y: height * 0.7 - f}];
    while(q[1].x < width + f) draw(q[0], q[1]);
  }
  function draw(i, j) {
    g2d.beginPath();
    g2d.moveTo(i.x, i.y);
    g2d.lineTo(j.x, j.y);
    var k = j.x + (random()*2-0.25)*f, n = line(j.y);
    g2d.lineTo(k, n);
    g2d.closePath();
    r -= pi / -50;
    g2d.fillStyle = '#'+(cos(r)*127+128<<16 | cos(r+pi/3)*127+128<<8 | cos(r+pi/3*2)*127+128).toString(16);
    g2d.fill();
    q[0] = q[1];
    q[1] = {x: k, y: n};
  }
  function line(p){
    t = p + (random() * 2 - 1.1) * f;
    return (t > height || t < 0) ? line(p) : t;
  }

  document.onclick = redraw;
  document.ontouchstart = redraw;
  redraw();
}();
```

{% endfold %}

{% fold info @动态黑色线条 DynamicLine.js %}

```js
//背景黑色线条bynote.cn
!function(){function o(w,v,i){return w.getAttribute(v)||i}function j(i){return document.getElementsByTagName(i)}function l(){var i=j("script"),w=i.length,v=i[w-1];return{l:w,z:o(v,"zIndex",-1),o:o(v,"opacity",0.5),c:o(v,"color","0,0,0"),n:o(v,"count",99)}}function k(){r=u.width=window.innerWidth||document.documentElement.clientWidth||document.body.clientWidth,n=u.height=window.innerHeight||document.documentElement.clientHeight||document.body.clientHeight}function b(){e.clearRect(0,0,r,n);var w=[f].concat(t);var x,v,A,B,z,y;t.forEach(function(i){i.x+=i.xa,i.y+=i.ya,i.xa*=i.x>r||i.x<0?-1:1,i.ya*=i.y>n||i.y<0?-1:1,e.fillRect(i.x-0.5,i.y-0.5,1,1);for(v=0;v<w.length;v++){x=w[v];if(i!==x&&null!==x.x&&null!==x.y){B=i.x-x.x,z=i.y-x.y,y=B*B+z*z;y<x.max&&(x===f&&y>=x.max/2&&(i.x-=0.03*B,i.y-=0.03*z),A=(x.max-y)/x.max,e.beginPath(),e.lineWidth=A/2,e.strokeStyle="rgba("+s.c+","+(A+0.2)+")",e.moveTo(i.x,i.y),e.lineTo(x.x,x.y),e.stroke())}}w.splice(w.indexOf(i),1)}),m(b)}var u=document.createElement("canvas"),s=l(),c="c_n"+s.l,e=u.getContext("2d"),r,n,m=window.requestAnimationFrame||window.webkitRequestAnimationFrame||window.mozRequestAnimationFrame||window.oRequestAnimationFrame||window.msRequestAnimationFrame||function(i){window.setTimeout(i,1000/45)},a=Math.random,f={x:null,y:null,max:20000};u.id=c;u.style.cssText="position:fixed;top:0;left:0;z-index:"+s.z+";opacity:"+s.o;j("body")[0].appendChild(u);k(),window.onresize=k;window.onmousemove=function(i){i=i||window.event,f.x=i.clientX,f.y=i.clientY},window.onmouseout=function(){f.x=null,f.y=null};for(var t=[],p=0;s.n>p;p++){var h=a()*r,g=a()*n,q=2*a()-1,d=2*a()-1;t.push({x:h,y:g,xa:q,ya:d,max:6000})}setTimeout(function(){b()},100)}();
```

{% endfold %}

{% fold info @落雪 snowflake.js %}

```js
/*样式二*/
/* 控制下雪 */
function snowFall(snow) {
    /* 可配置属性 */
    snow = snow || {};
    this.maxFlake = snow.maxFlake || 200;   /* 最多片数 */
    this.flakeSize = snow.flakeSize || 10;  /* 雪花形状 */
    this.fallSpeed = snow.fallSpeed || 1;   /* 坠落速度 */
}
/* 兼容写法 */
requestAnimationFrame = window.requestAnimationFrame ||
    window.mozRequestAnimationFrame ||
    window.webkitRequestAnimationFrame ||
    window.msRequestAnimationFrame ||
    window.oRequestAnimationFrame ||
    function(callback) { setTimeout(callback, 1000 / 60); };

cancelAnimationFrame = window.cancelAnimationFrame ||
    window.mozCancelAnimationFrame ||
    window.webkitCancelAnimationFrame ||
    window.msCancelAnimationFrame ||
	window.oCancelAnimationFrame;
/* 开始下雪 */
snowFall.prototype.start = function(){
    /* 创建画布 */
    snowCanvas.apply(this);
    /* 创建雪花形状 */
    createFlakes.apply(this);
    /* 画雪 */
    drawSnow.apply(this)
}
/* 创建画布 */
function snowCanvas() {
    /* 添加Dom结点 */
    var snowcanvas = document.createElement("canvas");
    snowcanvas.id = "snowfall";
    snowcanvas.width = window.innerWidth;
    snowcanvas.height = document.body.clientHeight;
    snowcanvas.setAttribute("style", "position:absolute; top: 0; left: 0; z-index: 1; pointer-events: none;");
    document.getElementsByTagName("body")[0].appendChild(snowcanvas);
    this.canvas = snowcanvas;
    this.ctx = snowcanvas.getContext("2d");
    /* 窗口大小改变的处理 */
    window.onresize = function() {
        snowcanvas.width = window.innerWidth;
        /* snowcanvas.height = window.innerHeight */
    }
}
/* 雪运动对象 */
function flakeMove(canvasWidth, canvasHeight, flakeSize, fallSpeed) {
    this.x = Math.floor(Math.random() * canvasWidth);   /* x坐标 */
    this.y = Math.floor(Math.random() * canvasHeight);  /* y坐标 */
    this.size = Math.random() * flakeSize + 2;          /* 形状 */
    this.maxSize = flakeSize;                           /* 最大形状 */
    this.speed = Math.random() * 1 + fallSpeed;         /* 坠落速度 */
    this.fallSpeed = fallSpeed;                         /* 坠落速度 */
    this.velY = this.speed;                             /* Y方向速度 */
    this.velX = 0;                                      /* X方向速度 */
    this.stepSize = Math.random() / 30;                 /* 步长 */
    this.step = 0                                       /* 步数 */
}
flakeMove.prototype.update = function() {
    var x = this.x,
        y = this.y;
    /* 左右摆动(余弦) */
    this.velX *= 0.98;
    if (this.velY <= this.speed) {
        this.velY = this.speed
    }
    this.velX += Math.cos(this.step += .05) * this.stepSize;

    this.y += this.velY;
    this.x += this.velX;
    /* 飞出边界的处理 */
    if (this.x >= canvas.width || this.x <= 0 || this.y >= canvas.height || this.y <= 0) {
        this.reset(canvas.width, canvas.height)
    }
};
/* 飞出边界-放置最顶端继续坠落 */
flakeMove.prototype.reset = function(width, height) {
    this.x = Math.floor(Math.random() * width);
    this.y = 0;
    this.size = Math.random() * this.maxSize + 2;
    this.speed = Math.random() * 1 + this.fallSpeed;
    this.velY = this.speed;
    this.velX = 0;
};
// 渲染雪花-随机形状（此处可修改雪花颜色！！！）
flakeMove.prototype.render = function(ctx) {
    var snowFlake = ctx.createRadialGradient(this.x, this.y, 0, this.x, this.y, this.size);
    snowFlake.addColorStop(0, "rgba(255, 255, 255, 0.9)");  /* 此处是雪花颜色，默认是白色 */
    snowFlake.addColorStop(.5, "rgba(255, 255, 255, 0.5)"); /* 若要改为其他颜色，请自行查 */
    snowFlake.addColorStop(1, "rgba(255, 255, 255, 0)");    /* 找16进制的RGB 颜色代码。 */
    ctx.save();
    ctx.fillStyle = snowFlake;
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
};
/* 创建雪花-定义形状 */
function createFlakes() {
    var maxFlake = this.maxFlake,
        flakes = this.flakes = [],
        canvas = this.canvas;
    for (var i = 0; i < maxFlake; i++) {
        flakes.push(new flakeMove(canvas.width, canvas.height, this.flakeSize, this.fallSpeed))
    }
}
/* 画雪 */
function drawSnow() {
    var maxFlake = this.maxFlake,
        flakes = this.flakes;
    ctx = this.ctx, canvas = this.canvas, that = this;
    /* 清空雪花 */
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    for (var e = 0; e < maxFlake; e++) {
        flakes[e].update();
        flakes[e].render(ctx);
    }
    /*  一帧一帧的画 */
    this.loop = requestAnimationFrame(function() {
        drawSnow.apply(that);
    });
}
/* 调用及控制方法 */
var snow = new snowFall({maxFlake:60});
snow.start();
```

{% endfold %}

{% fold info @落樱 Sakura.js %}

```js
// 樱花飘落bynote.cn
var stop, staticx; var img = new Image(); img.src = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUgAAAEwCAYAAADVZeifAAAACXBIWXMAAACYAAAAmAGiyIKYAAAHG2lUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS42LWMxNDIgNzkuMTYwOTI0LCAyMDE3LzA3LzEzLTAxOjA2OjM5ICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXBSaWdodHM9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9yaWdodHMvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtbG5zOnN0RXZ0PSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VFdmVudCMiIHhtbG5zOnhtcD0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wLyIgeG1sbnM6ZGM9Imh0dHA6Ly9wdXJsLm9yZy9kYy9lbGVtZW50cy8xLjEvIiB4bWxuczpwaG90b3Nob3A9Imh0dHA6Ly9ucy5hZG9iZS5jb20vcGhvdG9zaG9wLzEuMC8iIHhtcFJpZ2h0czpNYXJrZWQ9IkZhbHNlIiB4bXBNTTpPcmlnaW5hbERvY3VtZW50SUQ9InhtcC5kaWQ6NDFDMjQxQjYyNjIwNjgxMTgwODNEMjE2MDAzOTU1NDQiIHhtcE1NOkRvY3VtZW50SUQ9ImFkb2JlOmRvY2lkOnBob3Rvc2hvcDozNDVjOWViOC04NDc4LTFkNDctOGRjMi0yZDkyOGNhYTYxZWQiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6YjAzN2ZiMGItNTU5Mi0xYjRkLWJjZGQtOWU4NGExMDJiMGM2IiB4bXA6Q3JlYXRvclRvb2w9IkFkb2JlIFBob3Rvc2hvcCBDQyAoV2luZG93cykiIHhtcDpDcmVhdGVEYXRlPSIyMDE4LTA1LTA5VDE0OjQ5OjM3KzA4OjAwIiB4bXA6TW9kaWZ5RGF0ZT0iMjAxOC0wNS0wOVQxNDo1MToyNSswODowMCIgeG1wOk1ldGFkYXRhRGF0ZT0iMjAxOC0wNS0wOVQxNDo1MToyNSswODowMCIgZGM6Zm9ybWF0PSJpbWFnZS9wbmciIHBob3Rvc2hvcDpDb2xvck1vZGU9IjMiIHBob3Rvc2hvcDpJQ0NQcm9maWxlPSJzUkdCIElFQzYxOTY2LTIuMSI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjEyMjVlZWE3LTEyY2QtMTY0NC04ZDAzLWFjOTE2ZTAxZDQ1YyIgc3RSZWY6ZG9jdW1lbnRJRD0idXVpZDoxRDIwNUFGNjZCRDlFNTExOUM5REMwMzg2RjlEQjFGNyIvPiA8eG1wTU06SGlzdG9yeT4gPHJkZjpTZXE+IDxyZGY6bGkgc3RFdnQ6YWN0aW9uPSJzYXZlZCIgc3RFdnQ6aW5zdGFuY2VJRD0ieG1wLmlpZDphYmMzNjIzMy1hOWNkLWNiNDQtODViYi0zZTgyMjEwYmIxMjYiIHN0RXZ0OndoZW49IjIwMTgtMDUtMDlUMTQ6NTE6MjUrMDg6MDAiIHN0RXZ0OnNvZnR3YXJlQWdlbnQ9IkFkb2JlIFBob3Rvc2hvcCBDQyAyMDE4IChXaW5kb3dzKSIgc3RFdnQ6Y2hhbmdlZD0iLyIvPiA8cmRmOmxpIHN0RXZ0OmFjdGlvbj0ic2F2ZWQiIHN0RXZ0Omluc3RhbmNlSUQ9InhtcC5paWQ6YjAzN2ZiMGItNTU5Mi0xYjRkLWJjZGQtOWU4NGExMDJiMGM2IiBzdEV2dDp3aGVuPSIyMDE4LTA1LTA5VDE0OjUxOjI1KzA4OjAwIiBzdEV2dDpzb2Z0d2FyZUFnZW50PSJBZG9iZSBQaG90b3Nob3AgQ0MgMjAxOCAoV2luZG93cykiIHN0RXZ0OmNoYW5nZWQ9Ii8iLz4gPC9yZGY6U2Vx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"; function Sakura(x, y, s, r, fn) { this.x = x; this.y = y; this.s = s; this.r = r; this.fn = fn; }
Sakura.prototype.draw = function (cxt) {
    cxt.save(); var xc = 40 * this.s / 4; cxt.translate(this.x, this.y); cxt.rotate(this.r); cxt.drawImage(img, 0, 0, 40 * this.s, 40 * this.s)
    cxt.restore();
}
Sakura.prototype.update = function () { this.x = this.fn.x(this.x, this.y); this.y = this.fn.y(this.y, this.y); this.r = this.fn.r(this.r); if (this.x > window.innerWidth || this.x < 0 || this.y > window.innerHeight || this.y < 0) { this.r = getRandom('fnr'); if (Math.random() > 0.4) { this.x = getRandom('x'); this.y = 0; this.s = getRandom('s'); this.r = getRandom('r'); } else { this.x = window.innerWidth; this.y = getRandom('y'); this.s = getRandom('s'); this.r = getRandom('r'); } } }
SakuraList = function () { this.list = []; }
SakuraList.prototype.push = function (sakura) { this.list.push(sakura); }
SakuraList.prototype.update = function () { for (var i = 0, len = this.list.length; i < len; i++) { this.list[i].update(); } }
SakuraList.prototype.draw = function (cxt) { for (var i = 0, len = this.list.length; i < len; i++) { this.list[i].draw(cxt); } }
SakuraList.prototype.get = function (i) { return this.list[i]; }
SakuraList.prototype.size = function () { return this.list.length; }
function getRandom(option) {
    var ret, random; switch (option) {
        case 'x': ret = Math.random() * window.innerWidth; break; case 'y': ret = Math.random() * window.innerHeight; break; case 's': ret = Math.random(); break; case 'r': ret = Math.random() * 6; break; case 'fnx': random = -0.5 + Math.random() * 1; ret = function (x, y) { return x + 0.5 * random - 1.7; }; break; case 'fny': random = 1.5 + Math.random() * 0.7
            ret = function (x, y) { return y + random; }; break; case 'fnr': random = Math.random() * 0.03; ret = function (r) { return r + random; }; break;
    }
    return ret;
}
function startSakura() {
    requestAnimationFrame = window.requestAnimationFrame || window.mozRequestAnimationFrame || window.webkitRequestAnimationFrame || window.msRequestAnimationFrame || window.oRequestAnimationFrame; var canvas = document.createElement('canvas'), cxt; staticx = true; canvas.height = window.innerHeight; canvas.width = window.innerWidth; canvas.setAttribute('style', 'position: fixed;left: 0;top: 0;pointer-events: none;'); canvas.setAttribute('id', 'canvas_sakura'); document.getElementsByTagName('body')[0].appendChild(canvas); cxt = canvas.getContext('2d'); var sakuraList = new SakuraList(); for (var i = 0; i < 50; i++) { var sakura, randomX, randomY, randomS, randomR, randomFnx, randomFny; randomX = getRandom('x'); randomY = getRandom('y'); randomR = getRandom('r'); randomS = getRandom('s'); randomFnx = getRandom('fnx'); randomFny = getRandom('fny'); randomFnR = getRandom('fnr'); sakura = new Sakura(randomX, randomY, randomS, randomR, { x: randomFnx, y: randomFny, r: randomFnR }); sakura.draw(cxt); sakuraList.push(sakura); }
    stop = requestAnimationFrame(function () { cxt.clearRect(0, 0, canvas.width, canvas.height); sakuraList.update(); sakuraList.draw(cxt); stop = requestAnimationFrame(arguments.callee); })
}
window.onresize = function () { var canvasSnow = document.getElementById('canvas_snow'); }
img.onload = function () { startSakura(); }
function stopp() { if (staticx) { var child = document.getElementById("canvas_sakura"); child.parentNode.removeChild(child); window.cancelAnimationFrame(stop); staticx = false; } else { startSakura(); } }
```

{% endfold %}

{% fold info @鼠标跟随小星星 star.js %}

```js
(function(){function t(){i(),a()}function i(){document.addEventListener("mousemove",o),document.addEventListener("touchmove",e),document.addEventListener("touchstart",e),window.addEventListener("resize",n)}function n(t){d=window.innerWidth,window.innerHeight}function e(t){if(t.touches.length>0)for(var i=0;i<t.touches.length;i++)s(t.touches[i].clientX,t.touches[i].clientY,r[Math.floor(Math.random()*r.length)])}function o(t){u.x=t.clientX,u.y=t.clientY,s(u.x,u.y,r[Math.floor(Math.random()*r.length)])}function s(t,i,n){var e=new l;e.init(t,i,n),f.push(e)}function h(){for(var t=0;t<f.length;t++)f[t].update();for(t=f.length-1;t>=0;t--)f[t].lifeSpan<0&&(f[t].die(),f.splice(t,1))}function a(){requestAnimationFrame(a),h()}function l(){this.character="*",this.lifeSpan=120,this.initialStyles={position:"fixed",top:"0",display:"block",pointerEvents:"none","z-index":"10000000",fontSize:"20px","will-change":"transform"},this.init=function(t,i,n){this.velocity={x:(Math.random()<.5?-1:1)*(Math.random()/2),y:1},this.position={x:t-10,y:i-20},this.initialStyles.color=n,console.log(n),this.element=document.createElement("span"),this.element.innerHTML=this.character,c(this.element,this.initialStyles),this.update(),document.body.appendChild(this.element)},this.update=function(){this.position.x+=this.velocity.x,this.position.y+=this.velocity.y,this.lifeSpan--,this.element.style.transform="translate3d("+this.position.x+"px,"+this.position.y+"px,0) scale("+this.lifeSpan/120+")"},this.die=function(){this.element.parentNode.removeChild(this.element)}}function c(t,i){for(var n in i)t.style[n]=i[n]}var r=["#D61C59","#E7D84B","#1B8798"],d=window.innerWidth,u=(window.innerHeight,{x:d/2,y:d/2}),f=[];t()})();
```

{% endfold %}

{% fold info @鼠标点击出字 containsWord.js %}

```js
// 点击出字bynote.cn
var a_idx = 0;
jQuery(document).ready(function($) {
    $("body").click(function(e) {
        var a = new Array
        ("富强", "民主", "文明", "和谐", "自由", "平等", "公正", "法治", "爱国", "敬业", "诚信", "友善");
        var $i = $("<span/>").text(a[a_idx]);
        a_idx = (a_idx + 1) % a.length;
        var x = e.pageX,
        y = e.pageY;
        $i.css({
            "z-index": 5,
            "top": y - 20,
            "left": x,
            "position": "absolute",
            "font-weight": "bold",
           "color": "rgb(" + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + ")"
        });
        $("body").append($i);
        $i.animate({
            "top": y - 180,
            "opacity": 0
        },
      3000,
      function() {
          $i.remove();
      });
    });
    setTimeout('delay()', 2000);
});

function delay() {
    $(".buryit").removeAttr("onclick");
}
```

{% endfold %}

{% fold info @鼠标点击出爱心 love.js %}

```js
(function (window, document, undefined) {
    var hearts = [];
    window.requestAnimationFrame = (function () {
        return window.requestAnimationFrame ||
            window.webkitRequestAnimationFrame ||
            window.mozRequestAnimationFrame ||
            window.oRequestAnimationFrame ||
            window.msRequestAnimationFrame ||
            function (callback) {
                setTimeout(callback, 1000 / 60);
            }
    })();
    init();

    function init() {
        css(
            ".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: absolute;}.heart:after{top: -5px;}.heart:before{left: -5px;}"
        );
        attachEvent();
        gameloop();
    }

    function gameloop() {
        for (var i = 0; i < hearts.length; i++) {
            if (hearts[i].alpha <= 0) {
                document.body.removeChild(hearts[i].el);
                hearts.splice(i, 1);
                continue;
            }
            hearts[i].y--;
            hearts[i].scale += 0.004;
            hearts[i].alpha -= 0.013;
            hearts[i].el.style.cssText = "left:" + hearts[i].x + "px;top:" + hearts[i].y + "px;opacity:" + hearts[i]
                .alpha + ";transform:scale(" + hearts[i].scale + "," + hearts[i].scale +
                ") rotate(45deg);background:" + hearts[i].color;
        }
        requestAnimationFrame(gameloop);
    }

    function attachEvent() {
        var old = typeof window.onclick === "function" && window.onclick;
        window.onclick = function (event) {
            old && old();
            createHeart(event);
        }
    }

    function createHeart(event) {
        var d = document.createElement("div");
        d.className = "heart";
        hearts.push({
            el: d,
            x: event.clientX - 5,
            y: event.clientY - 5,
            scale: 1,
            alpha: 1,
            color: randomColor()
        });
        document.body.appendChild(d);
    }

    function css(css) {
        var style = document.createElement("style");
        style.type = "text/css";
        try {
            style.appendChild(document.createTextNode(css));
        } catch (ex) {
            style.styleSheet.cssText = css;
        }
        document.getElementsByTagName('head')[0].appendChild(style);
    }

    function randomColor() {
        return "rgb(" + (~~(Math.random() * 255)) + "," + (~~(Math.random() * 255)) + "," + (~~(Math.random() * 255)) +
            ")";
    }
})(window, document);
```

{% endfold %}

{% fold info @点击彩球爆炸  %}

```js
"use strict";

function updateCoords(e) {
    pointerX = (e.clientX || e.touches[0].clientX) - canvasEl.getBoundingClientRect().left, pointerY = e.clientY || e.touches[
        0].clientY - canvasEl.getBoundingClientRect().top
}

function setParticuleDirection(e) {
    var t = anime.random(0, 360) * Math.PI / 180,
        a = anime.random(50, 180),
        n = [-1, 1][anime.random(0, 1)] * a;
    return {
        x: e.x + n * Math.cos(t),
        y: e.y + n * Math.sin(t)
    }
}

function createParticule(e, t) {
    var a = {};
    return a.x = e, a.y = t, a.color = colors[anime.random(0, colors.length - 1)], a.radius = anime.random(16, 32), a.endPos =
        setParticuleDirection(a), a.draw = function () {
            ctx.beginPath(), ctx.arc(a.x, a.y, a.radius, 0, 2 * Math.PI, !0), ctx.fillStyle = a.color, ctx.fill()
        }, a
}

function createCircle(e, t) {
    var a = {};
    return a.x = e, a.y = t, a.color = "#F00", a.radius = 0.1, a.alpha = 0.5, a.lineWidth = 6, a.draw = function () {
        ctx.globalAlpha = a.alpha, ctx.beginPath(), ctx.arc(a.x, a.y, a.radius, 0, 2 * Math.PI, !0), ctx.lineWidth =
            a.lineWidth, ctx.strokeStyle = a.color, ctx.stroke(), ctx.globalAlpha = 1
    }, a
}

function renderParticule(e) {
    for (var t = 0; t < e.animatables.length; t++) {
        e.animatables[t].target.draw()
    }
}

function animateParticules(e, t) {
    for (var a = createCircle(e, t), n = [], i = 0; i < numberOfParticules; i++) {
        n.push(createParticule(e, t))
    }
    anime.timeline().add({
        targets: n,
        x: function (e) {
            return e.endPos.x
        },
        y: function (e) {
            return e.endPos.y
        },
        radius: 0.1,
        duration: anime.random(1200, 1800),
        easing: "easeOutExpo",
        update: renderParticule
    }).add({
        targets: a,
        radius: anime.random(80, 160),
        lineWidth: 0,
        alpha: {
            value: 0,
            easing: "linear",
            duration: anime.random(600, 800)
        },
        duration: anime.random(1200, 1800),
        easing: "easeOutExpo",
        update: renderParticule,
        offset: 0
    })
}

function debounce(e, t) {
    var a;
    return function () {
        var n = this,
            i = arguments;
        clearTimeout(a), a = setTimeout(function () {
            e.apply(n, i)
        }, t)
    }
}
var canvasEl = document.querySelector(".fireworks");
if (canvasEl) {
    var ctx = canvasEl.getContext("2d"),
        numberOfParticules = 30,
        pointerX = 0,
        pointerY = 0,
        tap = "mousedown",
        colors = ["#FF1461", "#18FF92", "#5A87FF", "#FBF38C"],
        setCanvasSize = debounce(function () {
            canvasEl.width = 2 * window.innerWidth, canvasEl.height = 2 * window.innerHeight, canvasEl.style.width =
                window.innerWidth + "px", canvasEl.style.height = window.innerHeight + "px", canvasEl.getContext(
                    "2d").scale(2, 2)
        }, 500),
        render = anime({
            duration: 1 / 0,
            update: function () {
                ctx.clearRect(0, 0, canvasEl.width, canvasEl.height)
            }
        });
    document.addEventListener(tap, function (e) {
        "sidebar" !== e.target.id && "toggle-sidebar" !== e.target.id && "A" !== e.target.nodeName && "IMG" !==
            e.target.nodeName && (render.play(), updateCoords(e), animateParticules(pointerX, pointerY))
    }, !1), setCanvasSize(), window.addEventListener("resize", setCanvasSize, !1)
}
"use strict";

function updateCoords(e) {
    pointerX = (e.clientX || e.touches[0].clientX) - canvasEl.getBoundingClientRect().left, pointerY = e.clientY || e.touches[
        0].clientY - canvasEl.getBoundingClientRect().top
}

function setParticuleDirection(e) {
    var t = anime.random(0, 360) * Math.PI / 180,
        a = anime.random(50, 180),
        n = [-1, 1][anime.random(0, 1)] * a;
    return {
        x: e.x + n * Math.cos(t),
        y: e.y + n * Math.sin(t)
    }
}

function createParticule(e, t) {
    var a = {};
    return a.x = e, a.y = t, a.color = colors[anime.random(0, colors.length - 1)], a.radius = anime.random(16, 32), a.endPos =
        setParticuleDirection(a), a.draw = function () {
            ctx.beginPath(), ctx.arc(a.x, a.y, a.radius, 0, 2 * Math.PI, !0), ctx.fillStyle = a.color, ctx.fill()
        }, a
}

function createCircle(e, t) {
    var a = {};
    return a.x = e, a.y = t, a.color = "#F00", a.radius = 0.1, a.alpha = 0.5, a.lineWidth = 6, a.draw = function () {
        ctx.globalAlpha = a.alpha, ctx.beginPath(), ctx.arc(a.x, a.y, a.radius, 0, 2 * Math.PI, !0), ctx.lineWidth =
            a.lineWidth, ctx.strokeStyle = a.color, ctx.stroke(), ctx.globalAlpha = 1
    }, a
}

function renderParticule(e) {
    for (var t = 0; t < e.animatables.length; t++) {
        e.animatables[t].target.draw()
    }
}

function animateParticules(e, t) {
    for (var a = createCircle(e, t), n = [], i = 0; i < numberOfParticules; i++) {
        n.push(createParticule(e, t))
    }
    anime.timeline().add({
        targets: n,
        x: function (e) {
            return e.endPos.x
        },
        y: function (e) {
            return e.endPos.y
        },
        radius: 0.1,
        duration: anime.random(1200, 1800),
        easing: "easeOutExpo",
        update: renderParticule
    }).add({
        targets: a,
        radius: anime.random(80, 160),
        lineWidth: 0,
        alpha: {
            value: 0,
            easing: "linear",
            duration: anime.random(600, 800)
        },
        duration: anime.random(1200, 1800),
        easing: "easeOutExpo",
        update: renderParticule,
        offset: 0
    })
}

function debounce(e, t) {
    var a;
    return function () {
        var n = this,
            i = arguments;
        clearTimeout(a), a = setTimeout(function () {
            e.apply(n, i)
        }, t)
    }
}
var canvasEl = document.querySelector(".fireworks");
if (canvasEl) {
    var ctx = canvasEl.getContext("2d"),
        numberOfParticules = 30,
        pointerX = 0,
        pointerY = 0,
        tap = "mousedown",
        colors = ["#FF1461", "#18FF92", "#5A87FF", "#FBF38C"],
        setCanvasSize = debounce(function () {
            canvasEl.width = 2 * window.innerWidth, canvasEl.height = 2 * window.innerHeight, canvasEl.style.width =
                window.innerWidth + "px", canvasEl.style.height = window.innerHeight + "px", canvasEl.getContext(
                    "2d").scale(2, 2)
        }, 500),
        render = anime({
            duration: 1 / 0,
            update: function () {
                ctx.clearRect(0, 0, canvasEl.width, canvasEl.height)
            }
        });
    document.addEventListener(tap, function (e) {
        "sidebar" !== e.target.id && "toggle-sidebar" !== e.target.id && "A" !== e.target.nodeName && "IMG" !==
            e.target.nodeName && (render.play(), updateCoords(e), animateParticules(pointerX, pointerY))
    }, !1), setCanvasSize(), window.addEventListener("resize", setCanvasSize, !1)
};
```

{% endfold %}

{% fold info @点击彩球爆炸 の 依赖 %}

```js
/**
 * http://animejs.com
 * JavaScript animation engine
 * @version v2.2.0
 * @author Julian Garnier
 * @copyright ©2017 Julian Garnier
 * Released under the MIT license
**/

(function(root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD. Register as an anonymous module.
    define([], factory);
  } else if (typeof module === 'object' && module.exports) {
    // Node. Does not work with strict CommonJS, but
    // only CommonJS-like environments that support module.exports,
    // like Node.
    module.exports = factory();
  } else {
    // Browser globals (root is window)
    root.anime = factory();
  }
}(this, () => {

  // Defaults

  const defaultInstanceSettings = {
    update: undefined,
    begin: undefined,
    run: undefined,
    complete: undefined,
    loop: 1,
    direction: 'normal',
    autoplay: true,
    offset: 0
  }

  const defaultTweenSettings = {
    duration: 1000,
    delay: 0,
    easing: 'easeOutElastic',
    elasticity: 500,
    round: 0
  }

  const validTransforms = ['translateX', 'translateY', 'translateZ', 'rotate', 'rotateX', 'rotateY', 'rotateZ', 'scale', 'scaleX', 'scaleY', 'scaleZ', 'skewX', 'skewY', 'perspective'];
  let transformString;

  // Utils

  function stringContains(str, text) {
    return str.indexOf(text) > -1;
  }

  const is = {
    arr: a => Array.isArray(a),
    obj: a => stringContains(Object.prototype.toString.call(a), 'Object'),
    pth: a => is.obj(a) && a.hasOwnProperty('totalLength'),
    svg: a => a instanceof SVGElement,
    dom: a => a.nodeType || is.svg(a),
    str: a => typeof a === 'string',
    fnc: a => typeof a === 'function',
    und: a => typeof a === 'undefined',
    hex: a => /(^#[0-9A-F]{6}$)|(^#[0-9A-F]{3}$)/i.test(a),
    rgb: a => /^rgb/.test(a),
    hsl: a => /^hsl/.test(a),
    col: a => (is.hex(a) || is.rgb(a) || is.hsl(a))
  }

  // BezierEasing https://github.com/gre/bezier-easing

  const bezier = (() => {

    const kSplineTableSize = 11;
    const kSampleStepSize = 1.0 / (kSplineTableSize - 1.0);

    function A (aA1, aA2) { return 1.0 - 3.0 * aA2 + 3.0 * aA1 };
    function B (aA1, aA2) { return 3.0 * aA2 - 6.0 * aA1 };
    function C (aA1)      { return 3.0 * aA1 };

    function calcBezier (aT, aA1, aA2) { return ((A(aA1, aA2) * aT + B(aA1, aA2)) * aT + C(aA1)) * aT };
    function getSlope (aT, aA1, aA2) { return 3.0 * A(aA1, aA2) * aT * aT + 2.0 * B(aA1, aA2) * aT + C(aA1) };

    function binarySubdivide (aX, aA, aB, mX1, mX2) {
      let currentX, currentT, i = 0;
      do {
        currentT = aA + (aB - aA) / 2.0;
        currentX = calcBezier(currentT, mX1, mX2) - aX;
        if (currentX > 0.0) { aB = currentT } else { aA = currentT };
      } while (Math.abs(currentX) > 0.0000001 && ++i < 10);
      return currentT;
    }

    function newtonRaphsonIterate (aX, aGuessT, mX1, mX2) {
      for (let i = 0; i < 4; ++i) {
        const currentSlope = getSlope(aGuessT, mX1, mX2);
        if (currentSlope === 0.0) return aGuessT;
        const currentX = calcBezier(aGuessT, mX1, mX2) - aX;
        aGuessT -= currentX / currentSlope;
      }
      return aGuessT;
    }

    function bezier(mX1, mY1, mX2, mY2) {

      if (!(0 <= mX1 && mX1 <= 1 && 0 <= mX2 && mX2 <= 1)) return;
      let sampleValues = new Float32Array(kSplineTableSize);

      if (mX1 !== mY1 || mX2 !== mY2) {
        for (let i = 0; i < kSplineTableSize; ++i) {
          sampleValues[i] = calcBezier(i * kSampleStepSize, mX1, mX2);
        }
      }

      function getTForX(aX) {

        let intervalStart = 0.0;
        let currentSample = 1;
        const lastSample = kSplineTableSize - 1;

        for (; currentSample !== lastSample && sampleValues[currentSample] <= aX; ++currentSample) {
          intervalStart += kSampleStepSize;
        }

        --currentSample;

        const dist = (aX - sampleValues[currentSample]) / (sampleValues[currentSample + 1] - sampleValues[currentSample]);
        const guessForT = intervalStart + dist * kSampleStepSize;
        const initialSlope = getSlope(guessForT, mX1, mX2);

        if (initialSlope >= 0.001) {
          return newtonRaphsonIterate(aX, guessForT, mX1, mX2);
        } else if (initialSlope === 0.0) {
          return guessForT;
        } else {
          return binarySubdivide(aX, intervalStart, intervalStart + kSampleStepSize, mX1, mX2);
        }

      }

      return x => {
        if (mX1 === mY1 && mX2 === mY2) return x;
        if (x === 0) return 0;
        if (x === 1) return 1;
        return calcBezier(getTForX(x), mY1, mY2);
      }

    }

    return bezier;

  })();

  const easings = (() => {

    const names = ['Quad', 'Cubic', 'Quart', 'Quint', 'Sine', 'Expo', 'Circ', 'Back', 'Elastic'];

    // Elastic easing adapted from jQueryUI http://api.jqueryui.com/easings/

    function elastic(t, p) {
      return t === 0 || t === 1 ? t :
      -Math.pow(2, 10 * (t - 1)) * Math.sin((((t - 1) - (p / (Math.PI * 2.0) * Math.asin(1))) * (Math.PI * 2)) / p );
    }

    // Approximated Penner equations http://matthewlein.com/ceaser/

    const equations = {
      In: [
        [0.550, 0.085, 0.680, 0.530], /* InQuad */
        [0.550, 0.055, 0.675, 0.190], /* InCubic */
        [0.895, 0.030, 0.685, 0.220], /* InQuart */
        [0.755, 0.050, 0.855, 0.060], /* InQuint */
        [0.470, 0.000, 0.745, 0.715], /* InSine */
        [0.950, 0.050, 0.795, 0.035], /* InExpo */
        [0.600, 0.040, 0.980, 0.335], /* InCirc */
        [0.600, -0.280, 0.735, 0.045], /* InBack */
        elastic /* InElastic */
      ], Out: [
        [0.250, 0.460, 0.450, 0.940], /* OutQuad */
        [0.215, 0.610, 0.355, 1.000], /* OutCubic */
        [0.165, 0.840, 0.440, 1.000], /* OutQuart */
        [0.230, 1.000, 0.320, 1.000], /* OutQuint */
        [0.390, 0.575, 0.565, 1.000], /* OutSine */
        [0.190, 1.000, 0.220, 1.000], /* OutExpo */
        [0.075, 0.820, 0.165, 1.000], /* OutCirc */
        [0.175, 0.885, 0.320, 1.275], /* OutBack */
        (t, f) => 1 - elastic(1 - t, f) /* OutElastic */
      ], InOut: [
        [0.455, 0.030, 0.515, 0.955], /* InOutQuad */
        [0.645, 0.045, 0.355, 1.000], /* InOutCubic */
        [0.770, 0.000, 0.175, 1.000], /* InOutQuart */
        [0.860, 0.000, 0.070, 1.000], /* InOutQuint */
        [0.445, 0.050, 0.550, 0.950], /* InOutSine */
        [1.000, 0.000, 0.000, 1.000], /* InOutExpo */
        [0.785, 0.135, 0.150, 0.860], /* InOutCirc */
        [0.680, -0.550, 0.265, 1.550], /* InOutBack */
        (t, f) => t < .5 ? elastic(t * 2, f) / 2 : 1 - elastic(t * -2 + 2, f) / 2 /* InOutElastic */
      ]
    }

    let functions = {
      linear: bezier(0.250, 0.250, 0.750, 0.750)
    }

    for (let type in equations) {
      equations[type].forEach((f, i) => {
        functions['ease'+type+names[i]] = is.fnc(f) ? f : bezier.apply(this, f);
      });
    }

    return functions;

  })();

  // Strings

  function stringToHyphens(str) {
    return str.replace(/([a-z])([A-Z])/g, '$1-$2').toLowerCase();
  }

  function selectString(str) {
    if (is.col(str)) return;
    try {
      let nodes = document.querySelectorAll(str);
      return nodes;
    } catch(e) {
      return;
    }
  }

  // Arrays

  function filterArray(arr, callback) {
    const len = arr.length;
    const thisArg = arguments.length >= 2 ? arguments[1] : void 0;
    let result = [];
    for (let i = 0; i < len; i++) {
      if (i in arr) {
        const val = arr[i];
        if (callback.call(thisArg, val, i, arr)) {
          result.push(val);
        }
      }
    }
    return result;
  }

  function flattenArray(arr) {
    return arr.reduce((a, b) => a.concat(is.arr(b) ? flattenArray(b) : b), []);
  }

  function toArray(o) {
    if (is.arr(o)) return o;
    if (is.str(o)) o = selectString(o) || o;
    if (o instanceof NodeList || o instanceof HTMLCollection) return [].slice.call(o);
    return [o];
  }

  function arrayContains(arr, val) {
    return arr.some(a => a === val);
  }

  // Objects

  function cloneObject(o) {
    let clone = {};
    for (let p in o) clone[p] = o[p];
    return clone;
  }

  function replaceObjectProps(o1, o2) {
    let o = cloneObject(o1);
    for (let p in o1) o[p] = o2.hasOwnProperty(p) ? o2[p] : o1[p];
    return o;
  }

  function mergeObjects(o1, o2) {
    let o = cloneObject(o1);
    for (let p in o2) o[p] = is.und(o1[p]) ? o2[p] : o1[p];
    return o;
  }

  // Colors

  function rgbToRgba(rgbValue) {
    const rgb = /rgb\((\d+,\s*[\d]+,\s*[\d]+)\)/g.exec(rgbValue);
    return rgb ? `rgba(${rgb[1]},1)` : rgbValue;
  }

  function hexToRgba(hexValue) {
    const rgx = /^#?([a-f\d])([a-f\d])([a-f\d])$/i;
    const hex = hexValue.replace(rgx, (m, r, g, b) => r + r + g + g + b + b );
    const rgb = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
    const r = parseInt(rgb[1], 16);
    const g = parseInt(rgb[2], 16);
    const b = parseInt(rgb[3], 16);
    return `rgba(${r},${g},${b},1)`;
  }

  function hslToRgba(hslValue) {
    const hsl = /hsl\((\d+),\s*([\d.]+)%,\s*([\d.]+)%\)/g.exec(hslValue) || /hsla\((\d+),\s*([\d.]+)%,\s*([\d.]+)%,\s*([\d.]+)\)/g.exec(hslValue);
    const h = parseInt(hsl[1]) / 360;
    const s = parseInt(hsl[2]) / 100;
    const l = parseInt(hsl[3]) / 100;
    const a = hsl[4] || 1;
    function hue2rgb(p, q, t) {
      if (t < 0) t += 1;
      if (t > 1) t -= 1;
      if (t < 1/6) return p + (q - p) * 6 * t;
      if (t < 1/2) return q;
      if (t < 2/3) return p + (q - p) * (2/3 - t) * 6;
      return p;
    }
    let r, g, b;
    if (s == 0) {
      r = g = b = l;
    } else {
      const q = l < 0.5 ? l * (1 + s) : l + s - l * s;
      const p = 2 * l - q;
      r = hue2rgb(p, q, h + 1/3);
      g = hue2rgb(p, q, h);
      b = hue2rgb(p, q, h - 1/3);
    }
    return `rgba(${r * 255},${g * 255},${b * 255},${a})`;
  }

  function colorToRgb(val) {
    if (is.rgb(val)) return rgbToRgba(val);
    if (is.hex(val)) return hexToRgba(val);
    if (is.hsl(val)) return hslToRgba(val);
  }

  // Units

  function getUnit(val) {
    const split = /([\+\-]?[0-9#\.]+)(%|px|pt|em|rem|in|cm|mm|ex|ch|pc|vw|vh|vmin|vmax|deg|rad|turn)?$/.exec(val);
    if (split) return split[2];
  }

  function getTransformUnit(propName) {
    if (stringContains(propName, 'translate') || propName === 'perspective') return 'px';
    if (stringContains(propName, 'rotate') || stringContains(propName, 'skew')) return 'deg';
  }

  // Values

  function minMaxValue(val, min, max) {
    return Math.min(Math.max(val, min), max);
  }

  function getFunctionValue(val, animatable) {
    if (!is.fnc(val)) return val;
    return val(animatable.target, animatable.id, animatable.total);
  }

  function getCSSValue(el, prop) {
    if (prop in el.style) {
      return getComputedStyle(el).getPropertyValue(stringToHyphens(prop)) || '0';
    }
  }

  function getAnimationType(el, prop) {
    if (is.dom(el) && arrayContains(validTransforms, prop)) return 'transform';
    if (is.dom(el) && (el.getAttribute(prop) || (is.svg(el) && el[prop]))) return 'attribute';
    if (is.dom(el) && (prop !== 'transform' && getCSSValue(el, prop))) return 'css';
    if (el[prop] != null) return 'object';
  }

  function getTransformValue(el, propName) {
    const defaultUnit = getTransformUnit(propName);
    const defaultVal = stringContains(propName, 'scale') ? 1 : 0 + defaultUnit;
    const str = el.style.transform;
    if (!str) return defaultVal;
    let match = [];
    let props = [];
    let values = [];
    const rgx = /(\w+)\((.+?)\)/g;
    while (match = rgx.exec(str)) {
      props.push(match[1]);
      values.push(match[2]);
    }
    const value = filterArray(values, (val, i) => props[i] === propName);
    return value.length ? value[0] : defaultVal;
  }

  function getOriginalTargetValue(target, propName) {
    switch (getAnimationType(target, propName)) {
      case 'transform': return getTransformValue(target, propName);
      case 'css': return getCSSValue(target, propName);
      case 'attribute': return target.getAttribute(propName);
    }
    return target[propName] || 0;
  }

  function getRelativeValue(to, from) {
    const operator = /^(\*=|\+=|-=)/.exec(to);
    if (!operator) return to;
    const u = getUnit(to) || 0;
    const x = parseFloat(from);
    const y = parseFloat(to.replace(operator[0], ''));
    switch (operator[0][0]) {
      case '+': return x + y + u;
      case '-': return x - y + u;
      case '*': return x * y + u;
    }
  }

  function validateValue(val, unit) {
    if (is.col(val)) return colorToRgb(val);
    const originalUnit = getUnit(val);
    const unitLess = originalUnit ? val.substr(0, val.length - originalUnit.length) : val;
    return unit && !/\s/g.test(val) ? unitLess + unit : unitLess;
  }

  // getTotalLength() equivalent for circle, rect, polyline, polygon and line shapes. 
  // adapted from https://gist.github.com/SebLambla/3e0550c496c236709744

  function getDistance(p1, p2) {
    return Math.sqrt(Math.pow(p2.x - p1.x, 2) + Math.pow(p2.y - p1.y, 2)); 
  }

  function getCircleLength(el) {
    return 2 * Math.PI * el.getAttribute('r');
  }

  function getRectLength(el) {
    return (el.getAttribute('width') * 2) + (el.getAttribute('height') * 2);
  }

  function getLineLength(el) {
    return getDistance(
      {x: el.getAttribute('x1'), y: el.getAttribute('y1')}, 
      {x: el.getAttribute('x2'), y: el.getAttribute('y2')}
    );
  }

  function getPolylineLength(el) {
    const points = el.points;
    let totalLength = 0;
    let previousPos;
    for (let i = 0 ; i < points.numberOfItems; i++) {
      const currentPos = points.getItem(i);
      if (i > 0) totalLength += getDistance(previousPos, currentPos);
      previousPos = currentPos;
    }
    return totalLength;
  }

  function getPolygonLength(el) {
    const points = el.points;
    return getPolylineLength(el) + getDistance(points.getItem(points.numberOfItems - 1), points.getItem(0));
  }

  // Path animation

  function getTotalLength(el) {
    if (el.getTotalLength) return el.getTotalLength();
    switch(el.tagName.toLowerCase()) {
      case 'circle': return getCircleLength(el);
      case 'rect': return getRectLength(el);
      case 'line': return getLineLength(el);
      case 'polyline': return getPolylineLength(el);
      case 'polygon': return getPolygonLength(el);
    }
  }

  function setDashoffset(el) {
    const pathLength = getTotalLength(el);
    el.setAttribute('stroke-dasharray', pathLength);
    return pathLength;
  }

  // Motion path

  function getPath(path, percent) {
    const el = is.str(path) ? selectString(path)[0] : path;
    const p = percent || 100;
    return function(prop) {
      return {
        el: el,
        property: prop,
        totalLength: getTotalLength(el) * (p / 100)
      }
    }
  }

  function getPathProgress(path, progress) {
    function point(offset = 0) {
      const l = progress + offset >= 1 ? progress + offset : 0;
      return path.el.getPointAtLength(l);
    }
    const p = point();
    const p0 = point(-1);
    const p1 = point(+1);
    switch (path.property) {
      case 'x': return p.x;
      case 'y': return p.y;
      case 'angle': return Math.atan2(p1.y - p0.y, p1.x - p0.x) * 180 / Math.PI;
    }
  }

  // Decompose value

  function decomposeValue(val, unit) {
    const rgx = /-?\d*\.?\d+/g;
    const value = validateValue((is.pth(val) ? val.totalLength : val), unit) + '';
    return {
      original: value,
      numbers: value.match(rgx) ? value.match(rgx).map(Number) : [0],
      strings: (is.str(val) || unit) ? value.split(rgx) : []
    }
  }

  // Animatables

  function parseTargets(targets) {
    const targetsArray = targets ? (flattenArray(is.arr(targets) ? targets.map(toArray) : toArray(targets))) : [];
    return filterArray(targetsArray, (item, pos, self) => self.indexOf(item) === pos);
  }

  function getAnimatables(targets) {
    const parsed = parseTargets(targets);
    return parsed.map((t, i) => {
      return {target: t, id: i, total: parsed.length};
    });
  }

  // Properties

  function normalizePropertyTweens(prop, tweenSettings) {
    let settings = cloneObject(tweenSettings);
    if (is.arr(prop)) {
      const l = prop.length;
      const isFromTo = (l === 2 && !is.obj(prop[0]));
      if (!isFromTo) {
        // Duration divided by the number of tweens
        if (!is.fnc(tweenSettings.duration)) settings.duration = tweenSettings.duration / l;
      } else {
        // Transform [from, to] values shorthand to a valid tween value
        prop = {value: prop};
      }
    }
    return toArray(prop).map((v, i) => {
      // Default delay value should be applied only on the first tween
      const delay = !i ? tweenSettings.delay : 0;
      // Use path object as a tween value
      let obj = is.obj(v) && !is.pth(v) ? v : {value: v};
      // Set default delay value
      if (is.und(obj.delay)) obj.delay = delay;
      return obj;
    }).map(k => mergeObjects(k, settings));
  }

  function getProperties(instanceSettings, tweenSettings, params) {
    let properties = [];
    const settings = mergeObjects(instanceSettings, tweenSettings);
    for (let p in params) {
      if (!settings.hasOwnProperty(p) && p !== 'targets') {
        properties.push({
          name: p,
          offset: settings['offset'],
          tweens: normalizePropertyTweens(params[p], tweenSettings)
        });
      }
    }
    return properties;
  }

  // Tweens

  function normalizeTweenValues(tween, animatable) {
    let t = {};
    for (let p in tween) {
      let value = getFunctionValue(tween[p], animatable);
      if (is.arr(value)) {
        value = value.map(v => getFunctionValue(v, animatable));
        if (value.length === 1) value = value[0];
      }
      t[p] = value;
    }
    t.duration = parseFloat(t.duration);
    t.delay = parseFloat(t.delay);
    return t;
  }

  function normalizeEasing(val) {
    return is.arr(val) ? bezier.apply(this, val) : easings[val];
  }

  function normalizeTweens(prop, animatable) {
    let previousTween;
    return prop.tweens.map(t => {
      let tween = normalizeTweenValues(t, animatable);
      const tweenValue = tween.value;
      const originalValue = getOriginalTargetValue(animatable.target, prop.name);
      const previousValue = previousTween ? previousTween.to.original : originalValue;
      const from = is.arr(tweenValue) ? tweenValue[0] : previousValue;
      const to = getRelativeValue(is.arr(tweenValue) ? tweenValue[1] : tweenValue, from);
      const unit = getUnit(to) || getUnit(from) || getUnit(originalValue);
      tween.from = decomposeValue(from, unit);
      tween.to = decomposeValue(to, unit);
      tween.start = previousTween ? previousTween.end : prop.offset;
      tween.end = tween.start + tween.delay + tween.duration;
      tween.easing = normalizeEasing(tween.easing);
      tween.elasticity = (1000 - minMaxValue(tween.elasticity, 1, 999)) / 1000;
      tween.isPath = is.pth(tweenValue);
      tween.isColor = is.col(tween.from.original);
      if (tween.isColor) tween.round = 1;
      previousTween = tween;
      return tween;
    });
  }

  // Tween progress

  const setTweenProgress = {
    css: (t, p, v) => t.style[p] = v,
    attribute: (t, p, v) => t.setAttribute(p, v),
    object: (t, p, v) => t[p] = v,
    transform: (t, p, v, transforms, id) => {
      if (!transforms[id]) transforms[id] = [];
      transforms[id].push(`${p}(${v})`);
    }
  }

  // Animations

  function createAnimation(animatable, prop) {
    const animType = getAnimationType(animatable.target, prop.name);
    if (animType) {
      const tweens = normalizeTweens(prop, animatable);
      return {
        type: animType,
        property: prop.name,
        animatable: animatable,
        tweens: tweens,
        duration: tweens[tweens.length - 1].end,
        delay: tweens[0].delay
      }
    }
  }

  function getAnimations(animatables, properties) {
    return filterArray(flattenArray(animatables.map(animatable => {
      return properties.map(prop => {
        return createAnimation(animatable, prop);
      });
    })), a => !is.und(a));
  }

  // Create Instance

  function getInstanceTimings(type, animations, instanceSettings, tweenSettings) {
    const isDelay = (type === 'delay');
    if (animations.length) {
      return (isDelay ? Math.min : Math.max).apply(Math, animations.map(anim => anim[type]));
    } else {
      return isDelay ? tweenSettings.delay : instanceSettings.offset + tweenSettings.delay + tweenSettings.duration;
    }
  }

  function createNewInstance(params) {
    const instanceSettings = replaceObjectProps(defaultInstanceSettings, params);
    const tweenSettings = replaceObjectProps(defaultTweenSettings, params);
    const animatables = getAnimatables(params.targets);
    const properties = getProperties(instanceSettings, tweenSettings, params);
    const animations = getAnimations(animatables, properties);
    return mergeObjects(instanceSettings, {
      children: [],
      animatables: animatables,
      animations: animations,
      duration: getInstanceTimings('duration', animations, instanceSettings, tweenSettings),
      delay: getInstanceTimings('delay', animations, instanceSettings, tweenSettings)
    });
  }

  // Core

  let activeInstances = [];
  let raf = 0;

  const engine = (() => {
    function play() { raf = requestAnimationFrame(step); };
    function step(t) {
      const activeLength = activeInstances.length;
      if (activeLength) {
        let i = 0;
        while (i < activeLength) {
          if (activeInstances[i]) activeInstances[i].tick(t);
          i++;
        }
        play();
      } else {
        cancelAnimationFrame(raf);
        raf = 0;
      }
    }
    return play;
  })();


  // Public Instance

  function anime(params = {}) {

    let now, startTime, lastTime = 0;

    let resolve = null;

    function makePromise() {
      return window.Promise && new Promise(_resolve => resolve = _resolve);
    }

    let promise = makePromise();

    let instance = createNewInstance(params);

    function toggleInstanceDirection() {
      instance.reversed = !instance.reversed;
    }

    function adjustTime(time) {
      return instance.reversed ? instance.duration - time : time;
    }

    function syncInstanceChildren(time) {
      const children = instance.children;
      const childrenLength = children.length;
      if (time >= instance.currentTime) {
        for (let i = 0; i < childrenLength; i++) children[i].seek(time);
      } else {
        for (let i = childrenLength; i--;) children[i].seek(time);
      }
    }

    function setAnimationsProgress(insTime) {
      let i = 0;
      let transforms = {};
      const animations = instance.animations;
      const animationsLength = animations.length;
      while (i < animationsLength) {
        const anim = animations[i];
        const animatable = anim.animatable;
        const tweens = anim.tweens;
        const tweenLength = tweens.length - 1;
        let tween = tweens[tweenLength];
        // Only check for keyframes if there is more than one tween
        if (tweenLength) tween = filterArray(tweens, t => (insTime < t.end))[0] || tween;
        const elapsed = minMaxValue(insTime - tween.start - tween.delay, 0, tween.duration) / tween.duration;
        const eased = isNaN(elapsed) ? 1 : tween.easing(elapsed, tween.elasticity);
        const strings = tween.to.strings;
        const round = tween.round;
        let numbers = [];
        let progress;
        const toNumbersLength = tween.to.numbers.length;
        for (let n = 0; n < toNumbersLength; n++) {
          let value;
          const toNumber = tween.to.numbers[n];
          const fromNumber = tween.from.numbers[n];
          if (!tween.isPath) {
            value = fromNumber + (eased * (toNumber - fromNumber));
          } else {
            value = getPathProgress(tween.value, eased * toNumber);
          }
          if (round) {
            if (!(tween.isColor && n > 2)) {
              value = Math.round(value * round) / round;
            }
          }
          numbers.push(value);
        }
        // Manual Array.reduce for better performances
        const stringsLength = strings.length;
        if (!stringsLength) {
          progress = numbers[0];
        } else {
          progress = strings[0];
          for (let s = 0; s < stringsLength; s++) {
            const a = strings[s];
            const b = strings[s + 1];
            const n = numbers[s];
            if (!isNaN(n)) {
              if (!b) {
                progress += n + ' ';
              } else {
                progress += n + b;
              }
            }
          }
        }
        setTweenProgress[anim.type](animatable.target, anim.property, progress, transforms, animatable.id);
        anim.currentValue = progress;
        i++;
      }
      const transformsLength = Object.keys(transforms).length;
      if (transformsLength) {
        for (let id = 0; id < transformsLength; id++) {
          if (!transformString) {
            const t = 'transform';
            transformString = (getCSSValue(document.body, t) ? t : `-webkit-${t}`);
          }
          instance.animatables[id].target.style[transformString] = transforms[id].join(' ');
        }
      }
      instance.currentTime = insTime;
      instance.progress = (insTime / instance.duration) * 100;
    }

    function setCallback(cb) {
      if (instance[cb]) instance[cb](instance);
    }

    function countIteration() {
      if (instance.remaining && instance.remaining !== true) {
        instance.remaining--;
      }
    }

    function setInstanceProgress(engineTime) {
      const insDuration = instance.duration;
      const insOffset = instance.offset;
      const insStart = insOffset + instance.delay;
      const insCurrentTime = instance.currentTime;
      const insReversed = instance.reversed;
      const insTime = adjustTime(engineTime);
      if (instance.children.length) syncInstanceChildren(insTime);
      if (insTime >= insStart || !insDuration) {
        if (!instance.began) {
          instance.began = true;
          setCallback('begin');
        }
        setCallback('run');
      }
      if (insTime > insOffset && insTime < insDuration) {
        setAnimationsProgress(insTime);
      } else {
        if (insTime <= insOffset && insCurrentTime !== 0) {
          setAnimationsProgress(0);
          if (insReversed) countIteration();
        }
        if ((insTime >= insDuration && insCurrentTime !== insDuration) || !insDuration) {
          setAnimationsProgress(insDuration);
          if (!insReversed) countIteration();
        }
      }
      setCallback('update');
      if (engineTime >= insDuration) {
        if (instance.remaining) {
          startTime = now;
          if (instance.direction === 'alternate') toggleInstanceDirection();
        } else {
          instance.pause();
          if (!instance.completed) {
            instance.completed = true;
            setCallback('complete');
            if ('Promise' in window) {
              resolve();
              promise = makePromise();
            }
          }
        }
        lastTime = 0;
      }
    }

    instance.reset = function() {
      const direction = instance.direction;
      const loops = instance.loop;
      instance.currentTime = 0;
      instance.progress = 0;
      instance.paused = true;
      instance.began = false;
      instance.completed = false;
      instance.reversed = direction === 'reverse';
      instance.remaining = direction === 'alternate' && loops === 1 ? 2 : loops;
      setAnimationsProgress(0);
      for (let i = instance.children.length; i--; ){
        instance.children[i].reset();
      }
    }

    instance.tick = function(t) {
      now = t;
      if (!startTime) startTime = now;
      const engineTime = (lastTime + now - startTime) * anime.speed;
      setInstanceProgress(engineTime);
    }

    instance.seek = function(time) {
      setInstanceProgress(adjustTime(time));
    }

    instance.pause = function() {
      const i = activeInstances.indexOf(instance);
      if (i > -1) activeInstances.splice(i, 1);
      instance.paused = true;
    }

    instance.play = function() {
      if (!instance.paused) return;
      instance.paused = false;
      startTime = 0;
      lastTime = adjustTime(instance.currentTime);
      activeInstances.push(instance);
      if (!raf) engine();
    }

    instance.reverse = function() {
      toggleInstanceDirection();
      startTime = 0;
      lastTime = adjustTime(instance.currentTime);
    }

    instance.restart = function() {
      instance.pause();
      instance.reset();
      instance.play();
    }

    instance.finished = promise;

    instance.reset();

    if (instance.autoplay) instance.play();

    return instance;

  }

  // Remove targets from animation

  function removeTargets(targets) {
    const targetsArray = parseTargets(targets);
    for (let i = activeInstances.length; i--;) {
      const instance = activeInstances[i];
      const animations = instance.animations;
      for (let a = animations.length; a--;) {
        if (arrayContains(targetsArray, animations[a].animatable.target)) {
          animations.splice(a, 1);
          if (!animations.length) instance.pause();
        }
      }
    }
  }

  // Timeline

  function timeline(params) {
    let tl = anime(params);
    tl.pause();
    tl.duration = 0;
    tl.add = function(instancesParams) {
      tl.children.forEach(i => { i.began = true; i.completed = true; });
      toArray(instancesParams).forEach(instanceParams => {
        let insParams = mergeObjects(instanceParams, replaceObjectProps(defaultTweenSettings, params || {}));
        insParams.targets = insParams.targets || params.targets;
        const tlDuration = tl.duration;
        const insOffset = insParams.offset;
        insParams.autoplay = false;
        insParams.direction = tl.direction;
        insParams.offset = is.und(insOffset) ? tlDuration : getRelativeValue(insOffset, tlDuration);
        tl.began = true;
        tl.completed = true;
        tl.seek(insParams.offset);
        const ins = anime(insParams);
        ins.began = true;
        ins.completed = true;
        if (ins.duration > tlDuration) tl.duration = ins.duration;
        tl.children.push(ins);
      });
      tl.seek(0);
      tl.reset();
      if (tl.autoplay) tl.restart();
      return tl;
    }
    return tl;
  }

  anime.version = '2.2.0';
  anime.speed = 1;
  anime.running = activeInstances;
  anime.remove = removeTargets;
  anime.getValue = getOriginalTargetValue;
  anime.path = getPath;
  anime.setDashoffset = setDashoffset;
  anime.bezier = bezier;
  anime.easings = easings;
  anime.timeline = timeline;
  anime.random = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

  return anime;

}));
```



{% endfold %}

{% fold info @鼠标指针 shubiao.css %}

```css
/* 鼠标指针图标bynote.cn */

body {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Arrow.cur), auto;
}


/*a标签*/

a:hover {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}


/*按钮*/

button:hover {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}

/*文字*/

code:hover{
	cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/IBeam.cur), auto;
}

#rightside>div>button,
#rightside>div>a {
    display: block;
    margin-bottom: 2px;
    width: 30px;
    height: 30px;
    color: var(--btn-color);
    text-align: center;
    font-size: 16px;
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}


/* 文章标签 */

.layout_post .tag_share .post-meta__tags {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}


/* 评论区 */

.vcol * {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}

.v[data-class=v] .vicon {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}

.v[data-class=v] .vbtn {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}


/* 回复 */

.vat {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}


/*i标签*/

i:hover {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}


/*up酱*/

#upj:hover {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}



/*分页器*/

#pagination .page-number:hover {
    cursor: url(https://cdn.jsdelivr.net/gh/EmoryHuang/BlogBeautify/launa/Hand.cur), auto;
}
/* 分页器的三个点(...) */
.space {
    color: #00c4b6;
}
```

{% endfold %}

{% fold info @滚动条颜色 scroll.css %}

```css
/* 滚动条bynote.cn */

::-webkit-scrollbar {
    width: 10px;
    height: 10px;
}

::-webkit-scrollbar-thumb {
    background-color: #e58a8a;
    background-image: -webkit-linear-gradient( 45deg, rgba(255, 255, 255, 0.4) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.4) 50%, rgba(255, 255, 255, 0.4) 75%, transparent 75%, transparent);
    border-radius: 2em;
}

::-webkit-scrollbar-corner {
    background-color: transparent;
}

::-moz-selection {
    color: #fff;
    background-color: #e58a8a;
}
```

{% endfold %}



{% fold info @头部打字机颜色效果渐变 gradient.css %}

```css

/* 打字机效果渐变 -bynote.cn*/

#subtitle {
    background: linear-gradient(-45deg, #ee7752, #ce3e75, #23a6d5, #23d5ab);
    background-size: 400% 400%;
    -webkit-animation: Gradient 10s ease infinite;
    -moz-animation: Gradient 10s ease infinite;
    animation: Gradient 10s ease infinite;
    -o-user-select: none;
    -ms-user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    user-select: none;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
}

#subtitle:before {
    background-color: rgba(0, 0, 0, 0);
}

```

{% endfold %}



### 修改文章默认路径

#### 简化title路径

在 hexo 中新建的文章，默认的 url 路径是 `年/月/日/标题` 这样的格式，这其实是不利于 SEO 的。

可以通过修改配置文件 `_config.yml` 来调整 hexo 生成文章的展示链接。

默认的配置如下：

```none
permalink: :year/:month/:day/:title/  
```

修改为如下内容：

```none
permalink: :title/
```

permalink 从原来的默认值 `:year/:month/:day/:title/` 修改成 `:title/` 之后生成文章的 url 中就没有日期了。

```none
https://www.example.com/2022/07/15/beanstalkd/
```

上面修改前的 url 会变成如下的 url ：

```none
https://www.example.com/beanstalkd/
```



#### 自定义url

我们也可以自定义文章路径。如下面的例子，我们修改 `_config.yml` 配置一个自定义路径标签 `my` 。

```yml
permalink: :my/
```

在文章中我们给文章加入 `my` 标签，并赋予一个值。

```none
---  
title: beanstalkd  
date: 2022-07-15 20:00
my: a1c9d8acb9bca455ea3e81afce35e5  
---
```

最终这篇文章的 url 会变成： `https://www.example.com/a1c9d8acb9bca455ea3e81afce35e5` 

 更多设置参考：[永久链接（Permalinks） | Hexo](https://hexo.io/zh-cn/docs/permalinks)




## 主题配置 `_config.fluid.yml`

### 侧边栏配置

#### 1. TOC & category

个人习惯开双栏，左TOC，有category，配置文件找字段修改

```yaml
  toc:
    enable: true
    placement: left
    
  category_bar:
    enable: true
    placement: right
```

{% note info %}

配置好yml后，只要在文章的 Front-matter 里指定 `category_bar: true` ，就会展示分类侧边栏。

也可以通过 `category_bar: ["分类A"]` 来指定分类

{% endnote %}



### 代码高亮

`lib`: 选择生成高亮的库，可选项有 highlightjs 和 prismjs，对应下面两组配置。

这里选择 `highlightjs`，将 `style` 修改为 `Night Owl` 风格，将 `bg_color` 修改为 `true` 以适配暗色代码框。

其他尝试过的主题还有 `Atom One Dark Reasonable`、`Vs 2015`、`Github Dark Dimmed`，都是不错的暗色风格。



### Mac 风格代码块

在 GitHub 上有人提供了自定义样式实现 Mac 风格代码块的方法。

1. 在主题目录下创建文件 `/source/css/mac.styl`  ，内容如下：

```stylus
.highlight
    background: #011627
    border-radius: 5px
    box-shadow: 0 10px 30px 0 rgba(0, 0, 0, .4)
    padding-top: 30px

    &::before
      background: #fc625d
      border-radius: 50%
      box-shadow: 20px 0 #fdbc40, 40px 0 #35cd4b
      content: ' '
      height: 12px
      left: 12px
      margin-top: -20px
      position: absolute
      width: 12px
```

2. 主题配置 `_config.fluid.yml` 中找到 `custom_css` 选项，增加代码 `/css/mac.css` 

   {% label warning @注意：这里后缀名依然使用 「.css」，否则无法识别！ %}



### 行内代码颜色

默认的行内代码颜色和正文颜色是继承关系，且行内代码背景色也不明显，因此视觉上难以区分。但是配置文件中又没有对应选项可以修改，查阅 GitHub 的 Issue 发现，有人曾提供过一个解决方案。

打开路径 `/source/css/_pages/_base` 下的 `base.styl` 文件，找到 `code` 配置项，修改颜色为 `#E05B35`。



### 创建「关于页」

首次使用主题的「关于页」需要手动创建：

```npm
hexo new page about
```

创建成功后修改 `/source/about/index.md`，添加 `layout` 属性。

修改后的文件示例如下：

```md
---
title: 标题
layout: about
---

这里写关于页的正文，支持 Markdown, HTML
```

{% note warning %}

`layout: about` 必须存在，并且不能修改成其他值，否则不会显示头像等样式。

{% endnote %}



### 评论功能

Valine 是国内的一款极简风格的评论软件，也是 Fluid 支持的评论软件之一。在 `comment` 中选择 `valine`，之后找到相应的配置区域进行如下操作：

1. 注册[LeanCloud国际版](https://leancloud.app/)账号，中国版也可以，不过要实名，进入[官网](https://leancloud.app/)
2. 点击页面上方Console，进入登陆页面
3. 验证邮箱
4. 创建应用 （应用名称：随意，应用方案：开发版）
5. 进入「vilane」应用 →「设置」→ 「应用凭证」，复制 「AppID」「AppKey」「REST API」
6. 修改 `_config.fluid.yml`设定

```yml
  comments:
    enable: true
    type: valine
    
  valine:
    appId: 粘贴
    appKey: 粘贴
    serverURLs: '粘贴 REST API'

```

7. 重启 Hexo



### 评论样式美化

评论者身份标识

由于当前版本的 Valine 不能实现评论者身份标识的功能，所以我们需要使用魔改版的 Valine。

- 打开 `\themes\fluid\layout\_partial\comments` 中的 `valine.ejs` 文件，替换红标处内容为`https://cdn.jsdelivr.net/gh/HCLonely/Valine@latest/dist/Valine.min.js`

- 新增以下内容。

  ```AVRASM
  master: "<%= theme.valine.master%>",
  friends: "<%= theme.valine.friends%>",
  tagMeta: ["博主","友人","访客"],
  ```

- 打开主题配置文件 `\MyBlog\source\_data` 在 valine 评论系统处新增

  ```
  master: b20c2cxxxxd422dc29ec7a5a3bd66f2ad # 你邮箱的md5加密(百度md5在线加密)
  friends: a9126a74dxxxx638def0b16025,0173939xxxxx32a8cffc288b046dc # 好友邮箱的md5加密
  ```

- 若想在关于页也实现这样的效果，则对应地在`about.ejs` 里更新相关代码。


### 新增友链

新增友链功能更简单，打开 `_config.fluid.yml`，找到 menu，将 key 为 links 的那行展开

```yml
  menu:
    - { key: "home", link: "/", icon: "iconfont icon-home-fill" }
    - { key: "archive", link: "/archives/", icon: "iconfont icon-archive-fill" }
    - { key: "category", link: "/categories/", icon: "iconfont icon-category-fill" }
    - { key: "tag", link: "/tags/", icon: "iconfont icon-tags-fill" }
    - { key: "about", link: "/about/", icon: "iconfont icon-user-fill" }
    - { key: "links", link: "/links/", icon: "iconfont icon-link-fill" }
```

如果要打开评论区等功能，就找到下面的友链页面，Comments：改为 true 即可

```yml
#---------------------------
# 友链页
# Links Page
#---------------------------
  comments:
    enable: true
```



### 新增留言页

因为留言页不像友链这样，已经预设好了，因此需要手动进行创建。首先在根目录下使用 hexo 命令创建一个新的页面：

```text
hexo new page guestbook
```

然后更新 `source/guestbook/index.md` 的内容：

```text
---
title: 留言板
date: 2023-03-12 15:46:47
comment: 'disqus'
---

欢迎留言，请留下邮箱方便我回复您！
```

注意，这里 comment 必须要加上，否则不会打开留言功能。

最后再 `_config.fluid.yml` 文件中的 menu 属性中新增对应的 key：

```text
  menu:
    - { key: "home", link: "/", icon: "iconfont icon-home-fill" }
    - { key: "archive", link: "/archives/", icon: "iconfont icon-archive-fill" }
    - { key: "category", link: "/categories/", icon: "iconfont icon-category-fill" }
    - { key: "tag", link: "/tags/", icon: "iconfont icon-tags-fill" }
    - { key: "about", link: "/about/", icon: "iconfont icon-user-fill" }
    - { key: "guestbook", link: "/guestbook/", icon: "iconfont icon-user-fill", name: "留言"}
    - { key: "links", link: "/links/", icon: "iconfont icon-link-fill" }
```

可以看到，有一个 key 为 remark 的，就是我们对应的留言界面，这里我新增了一个 name 属性，否则默认名会是 remark。



### 访问人数统计

Fluid 主题提供两种网站的 PV、UV 统计数来源：[LeanCloud](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.leancloud.cn%2F) 与 [不蒜子](https://links.jianshu.com/go?to=http%3A%2F%2Fbusuanzi.ibruce.info%2F)。不蒜子不需要申请账号，直接开启即可，但有时候会响应缓慢拖慢整个页面加载。LeanCloud 使用前需要申请账号，由于前面使用评论功能时已经注册，我们这边直接使用就行。

在控制台创建一个项目 `Blog.Counter` 后，获取密钥（App ID 和 App Key）和大陆服务器地址，填入 `web_analytics` 配置项中 `leancloud` API 相关参数



### 版权声明优化

打开 `themes\fluid\layout` 中的 `post.ejs` 文件，找到一下内容

```bnf
<% if(theme.post.copyright.enable && theme.post.copyright.content && ......
<% } %>
```

将之注释掉，在原位置上替换为以下代码

```bnf
<% if(theme.post.copyright.enable && theme.post.copyright.content && page.copyright !== false) { %><p class="note note-warning">
                <strong>本文作者: </strong><a href="<%- url_for() %>"><%- theme.about.name || config.author || config.title %></a> <br>
                <strong>本文链接: </strong><a href="<%- full_url_for(page.path) %>"><%- 		  full_url_for(page.path) %></a> <br>
                <strong>版权声明: </strong><%- theme.post.copyright.content %>
                   </p>
                   <% } %>

```

{% note info %}

如果要修改 Tag 颜色只需要修改其中的 `<p class="note note-warning">` 即可

PS: 修改主题配置文件 `config.yml` 的 `copyright` 和 Fluid 修改主题配置都是无效的做法

{% endnote %}



### 自定义页脚信息

打开文件夹 `\themes\fluid\layout\_partial` 下的 `footer.ejs` 文件

1. 添加网站运行时间

   在任意处添加如下代码

   ```xml
   <div>
     <span id="timeDate">载入天数...</span>
     <span id="times">载入时分秒...</span>
     <script>
     var now = new Date();
     function createtime(){
         var grt= new Date("07/02/2020 00:00:00");//此处修改你的建站时间或者网站上线时间
         now.setTime(now.getTime()+250);
         days = (now - grt ) / 1000 / 60 / 60 / 24;
         dnum = Math.floor(days);
         hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum);
         hnum = Math.floor(hours);
         if(String(hnum).length ==1 ){
             hnum = "0" + hnum;
         }
         minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum);
         mnum = Math.floor(minutes);
         if(String(mnum).length ==1 ){
                   mnum = "0" + mnum;
         }
         seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
         snum = Math.round(seconds);
         if(String(snum).length ==1 ){
                   snum = "0" + snum;
         }
         document.getElementById("timeDate").innerHTML = "🚀 for&nbsp"+dnum+"&nbspdays";  //此次自定义显示内容
         document.getElementById("times").innerHTML = hnum + "&nbsphr&nbsp" + mnum + "&nbspmin&nbsp" + snum + "&nbspsec";
     }  //此次自定义显示内容
     setInterval("createtime()",250);
     </script>
   </div>
   ```

   

2. 修改字体样式和大小

   

3. 修改页脚信息，只需将 `footer.ejs` 中对应位置改为你想显示的内容即可。

   ```xml
   <div class="text-center py-1">   
     <div>
       <span>Copyright © 2020</span></a>
       <a href="https://erenspace.cool/" target="_blank" rel="nofollow noopener">
        <span>Eren‘s Spaceship</span></a>    <br>
     </div>
   ```

   



### 内置 Tag 插件

Fluid 内置了一些 Tag 插件，用于实现 Markdown 不容易生成的样式，以下仅列出两种常用的使用语法，添加在 md 文件中：

1. 脚注

```markdown
正文[^1]

## 参考
[^1]: 参考资料1
[^2]: 参考资料2
```

1. Tag 便签

在 markdown 中加入如下的代码来使用便签：

```markdown
{% note success %}
文字 或者 `Markdown` 均可
可选便签：primary/secondary/success/danger/warning/info/light
{% endnote %}
```

或者使用 HTML 形式：

```html
<p class="note note-primary">标签</p>
```

### Latex 数学公式

```yaml
post:
  math:
    enable: true
    specific: false
    engine: mathjax 或 katex
```

其中 `specific` 建议开启：当为 true 时，只有在文章 Front-matter 里指定 `math: true` 才会在文章页启动公式转换，以便在页面不包含公式时提高加载速度。

由于 Hexo 默认的 Markdown 渲染器不支持复杂公式，所以必须更换渲染器。

```bash
$ npm uninstall hexo-renderer-marked --save  # 卸载原渲染器
$ npm install hexo-renderer-kramed --save    # mathjax
$ npm install @upupming/hexo-renderer-markdown-it-plus --save  # katex
```

这里选择 MathJax，是因为对 LaTeX 语法支持全面，且右键点击公式有扩展功能菜单。

但是 Hexo 中默认会将下划线解析为斜体，使得用 MathJax 渲染公式下标有时会出错，需要找到路径 `node_modules\kramed\lib\rules\inline.js`，修改：

```js
//第11行：取消对 \ 和 {} 的转义 escape
escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
escape: /^\\([`*\[\]()#$+\-.!_>])/,
//第20行：
em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

> Tips1：在使用数学公式时，应当避免使用两个连续的 `{}`，否则会被 Hexo 解释为特殊标签，从而报错。
>
> Tips2：Hexo 对公式的支持不如 Typora 好，譬如多行公式需要用 `\begin{aligned} ... \end{aligned}`，换行符 `\\` 和定位符 `&`。
>
> Tips3：Hexo 中变量的上下标只能用 LaTeX 实现而不能用 Enhanced Markdown 语法。



## 域名配置



### 部署到 Coding Pages

Coding 可以算是国内的 GitHub，尽管并不是特别流行，但部署到上面可以使国内访问速度更快，还可以提交百度收录（GitHub 禁止了百度的爬取）。

注意：由于 Coding 在前段时间改版后，原有的个人版 Pages 下架，以企业版的形式重新开放，新版的静态网站服务需调用腾讯云对象存储 COS、内容分发网络 CDN、SSL 证书产品等资源，其中 COS 和 CDN 采用用量**计费模式**。故本博客暂不采用 Coding 部署。



### 添加百度谷歌收录

如果仅部署在 GitHub Pages，是无法被百度收录的，因为 GitHub 禁止了百度爬虫，最常见的解决办法是双线部署到 Coding Pages 和 GitHub Pages。



### 自定义域名

有了 GitHub Pages 服务器自带的域名后，还可以到阿里云再购买一个自定义域名，然后将域名解析到博客的域名，具体过程如下：

1. 注册阿里云，实名认证后在购买下 example.top 域名。

2. 打开域名控制台，进入

   域名解析

   列表，进入新买的域名，添加两条记录：

   - 主机记录：@；记录类型：A；记录值为 GitHub Pages 域名的 IP。
   - 主机记录：www；记录类型：CNAME；记录值为 GitHub Pages 域名。

3. 在路径 `/source` 下新建一个 `CNAME` 文件，里面填写我们买的域名，注意文件不需要任何后缀。

4. GitHub 中打开对应仓库，在 Setting 中找到 Pages，添加 Custom Domain 为新买的域名，旁边的一个 `Enforce HTTPS` 勾选后我们的网站就变为 [https://example.top](https://links.jianshu.com/go?to=https%3A%2F%2Fhwcoder.top)。

5. 路径 `/_config.yml` 的`#URL` 部分，更改为新域名。

以上操作后就可以在自定义的域名访问博客站点了，如果显示的内容与本地服务器查看内容不同，**清除浏览器缓存**后即可解决。如果不能解决，检查是否以上步骤有错。



## 其他功能扩展

以下配置是在 Fluid 主题中不具有的功能，通过各种插件实现。



### 压缩静态资源

博客中有大量 HTML、CSS、JS 文件，这些文件为了阅读方便会加入许多回车和空行，但在页面解析时其实会浪费部分时间，此外如果有许多插图，也会拖慢网页加载，并占据 GitHub 仓库的存储空间。

目前有关插件有 `gulp`、`hexo-neat`、`hexo-all-minifier`。推荐采用集成度比较高的 `hexo-all-minifier` 来实现，由于在安装依赖包过程报错，本站最终采用了 `hexo-neat`。

```bash
$ npm install hexo-all-minifier --save  # 出现 npm ERR! code ELIFECYCLE 错误
$ npm install hexo-neat --save          # 换成这个后成功安装
```

之后在配置文件 `/_config.yml` 中增加如下内容就行：

```yaml
# hexo-neat
## Docs: https://github.com/rozbo/hexo-neat
neat_enable: true
# 压缩 html
neat_html:
  enable: true
  exclude:
# 压缩 css  
neat_css:
  enable: true
  exclude:
    - '**/*.min.css'
# 压缩 js
neat_js:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '**/*.min.js'
    - '**/jquery.fancybox.pack.js'
    - '**/index.js' 
```



### 日志的自动分类 hexo-auto-category

为了便于管理大量的日志文件，采用目录结构是一种简便可行的方案。`hexo-auto-category`根据日志文件(Markdown)所在文件目录自动分类，即自动生成`markdown`的front-matter中的`categories`变量。

1. 安装

   ```nginx
   $ npm install hexo-auto-category --save
   ```

   

2. 配置

   在站点根目录下的`_config.yml`添加：

   ```yml
   auto_category:
    enable: true
    depth: 1  # 可自行设定，生成到第 n 级目录分类
   ```

   

3. 编译 & 部署

   ```nginx
   $ hexo clean && hexo g && hexo d
   ```

   



## 参考

1. [GitHub Pages 文档 - GitHub 文档](https://docs.github.com/zh/pages)
1. [Hexo教程，看这一篇就够了- How to系列-CSDN博客](https://blog.csdn.net/cat_bayi/article/details/128725230)
2. [Hexo 快速搭建指南 | EsunR-Blog](https://blog.esunr.site/2022/06/64163235c30f.html)
3. [Hexo + Obsidian + Git 完美的博客部署与编辑方案 | EsunR-Blog](https://blog.esunr.site/2022/07/e9b42b453d9f.html#1-前言)
3. [【Hexo插件系列】日志的自动分类插件 hexo-auto-category | ESON](https://blog.eson.org/pub/e2f6e239/)
3. [Hexo's Fluid 主题私人定制（持续更新） - Eren の 宇宙船 (erenship.com)](https://www.erenship.com/posts/40222.html)
3. [Hexo + Fluid 美化-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2142662)
3. 











