# 客户端检测
应对各个厂商、各个版本的浏览器对特性支持度的差异，常采取的策略

* “最小公分母” 策略
* 客户端检测技术，突破和规避种种局限

不到万不得已不要使用客户端检测；先设计最通用的方案，然后使用特定于浏览器的技术增强该方案。

常用的客户端检测技术包括：

* 能力检测
* 怪癖检测（较少用）
* 用户代理检测

## 能力检测
1. 最常用最方便，只关注能力，不关注浏览器品牌和版本；

2. 两个原则
    * 先检测达成目的的最常用特性，避免测试多个条件
    * 必须测试实际用到的特性，一个特性存在不一定代表另一个特性也存在

3. 在可能的情况下，尽量使用 `typeof` 进行能力检测；

4. 低版本 IE 下或其他非标准浏览器下，使用 `typeof` 检测的值不一定符合标准；
    > 低版本 IE 的 DOM 以 COM 的形式实现，很容易出现 `typeof` 检测结果不标准的情况

5. 通过检查对象是否有某些原生属性时，要注意我们的代码是否定义了与要检测的原生属性同名的属性，确保检测的目标是正确的；

6. 一般通用方法：`isHostMethod` 函数
    ```javascript
    function isHostMethod(object, property) {
        var t = typeof object[property];
        return t == 'function' || (!!(t == 'object' && object[property])) || t == 'unknown';
    }

    result = isHostMethod(document, 'getElementById');
    // true
    ```

7. 根据浏览器不同将能力组合起来是更可取的方法；

8. 如果你预先知道自己的程序需要使用某些特定的浏览器特性，那么最好一次性检测所有相关特性。

## 怪癖检测
1. 目的是识别浏览器的特殊行为，与能级检测的检测能力刚好相反；

2. 怪癖是个别浏览器独有的，通常归类为 bug；

3. 实例：IE 8 及更早版本中，如果某个实例属性与 [[Enumerable]] 标记为 false 的某个原型属性同名，那么该属性无法被 `for-in` 循环遍历
    ```javascript
    // 怪癖检测

    var hasDontEnumQuirk = function() {
        var o = {toString: function() {}};
        for (var prop in o) {
            if (prop == 'toString') {
                return false;
            }
        }

        return true;
    }();

    hasDontEnumQuirk;
    // false 说明浏览器没有这个怪癖
    ```

4. 实例：Safari 3 以前版本会枚举被隐藏的属性
    ```javascript
    // 怪癖检测

    var hasDontEnumQuirk = function() {
        var o = {toString: function() {}};
        var count = 0;
        for (var prop in o) {
            if (prop == 'toString') {
                count++;
            }
        }

        return (count > 1);
    }();

    hasDontEnumQuirk;
    // false 说明浏览器没有这个怪癖
    ```

## 用户代理检测
1. 通过检测用户代理字符串来确定实际使用的浏览器；

2. 通常是在最后才选择这样的方法，或者想实现某些特定的功能才使用用户代理检测；

3. 主要使用 `navigator.useragent` `navigator.platform` 和一些浏览器的特征对象来检测；

4. 检测主要内容是：呈现引擎信息（`engine`）、浏览器信息、平台、设备、操作系统；

5. 各个浏览器的用户代理字符串因为历史的原因十分复杂与怪异，具体见 P<sub>222</sub>

6. 代码
    ```javascript
    var client = function() {

        // 呈现引擎与版本
        var engine = {
            ie: 0,
            gecko: 0,
            webkit: 0,
            khtml: 0,
            opera: 0,

            // 呈现引擎版本号
            ver: null
        };

        // 浏览器
        var browser = {

            // 主要浏览器
            ie: 0,
            firefox: 0,
            safari: 0,
            konq: 0,
            opera: 0,
            chrome: 0,

            // 浏览器版本号
            ver: null
        };

        // 平台、设备和操作系统
        var system = {
            win: false,
            max: false,
            x11: false,

            // 移动设备
            iphone: false,
            ipod: false,
            ipad: false,
            android: false,
            nokiaN: false,
            winMobile: false,

            // 游戏系统
            wii: false,
            ps: false
        };

        // 检测呈现引擎和浏览器
        var ua = navigator.userAgent;
        if (window.opera) {
            engine.ver = browser.ver = window.opera.version();
            engine.opera = browser.opera = parseFloat(engine.ver);
        } else if (/AppleWebKit\/(\S+)/.test(ua)) {
            engine.ver = RegExp['$1'];
            engine.webkit = parseFloat(engine.ver);

            // 确定是 Chrome 还是 Safari 还是 Edge
            if (/Edge\/(\S+)/.test(ua)) {
                engine.ver = browser.ver = 'Edge';
                engine.ie = browser.ie = 'Edge';
            } else if (/Chrome\/(\S+)/.test(ua)) {
                browser.ver = RegExp['$1'];
                browser.chrome = parseFloat(browser.ver);
            } else if (/Version\/(\S+)/.test(ua)) {
                browser.ver = RegExp['$1'];
                browser.safari = parseFloat(browser.ver);
            } else {
                // 近似地确定版本号
                var safariVersion = 1;
                if (engine.webkit < 100) {
                    safariVersion = 1;
                } else if (engine.webkit < 312) {
                    safariVersion = 1.2;
                } else if (engine.webkit < 412) {
                    safariVersion = 1.3;
                } else {
                    safariVersion = 2;
                }

                browser.safari = browser.ver.safariVersion;
            }
        } else if (/KHTML\/(\S+)/.test(ua) || /Konqueror\/(\S+)/.test(ua)) {
            engine.ver = browser.ver = RegExp['$1'];
            engine.khtml = browser.konq = parseFloat(engine.ver);
        } else if (/rv:(\S+)+\) Gecko\/\d{8}/.test(ua)) {
            engine.ver = RegExp['$1'];
            engine.gecko = parseFloat(engine.ver);

            // 确定是不是 Firefox
            if (/Firefox\/(\S+)/.test(ua)) {
                browser.ver = RegExp['$1'];
                browser.firefox = parseFloat(engine.ver);
            }
        } else if (/MSIE ([^;]+)/.test(ua)) {
            engine.ver = browser.ver = RegExp['$1'];
            engine.ie = browser.ie = parseFloat(engine.ver);
        } else if (/rv:(\S+)+\) like Gecko/) {
            engine.ver = browser.ver = RegExp['$1'];
            engine.ie = browser.ie = parseFloat(engine.ver);
        }

        // 检测浏览器
        browser.ie = engine.ie;
        browser.opera = engine.opera;

        // 检测平台
        var p = navigator.platform;
        system.win = p.indexOf('Win') == 0;
        system.mac = p.indexOf('Mac') == 0;
        system.x11 = (p == 'X11') || (p.indexOf('Linux') == 0);

        // 检测 windows 操作系统
        if (system.win) {
            if (/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)) {
                if (RegExp['$1'] == 'NT') {
                    switch (RegExp['$2']) {
                        case '5.0':
                            system.win = '2000';
                            break;
                        case '5.1':
                            system.win = 'XP';
                            break;
                        case '6.0':
                            system.win = 'Vista';
                            break;
                        case '6.1':
                            system.win = '7';
                            break;
                        case '6.2':
                            system.win = '8';
                            break;
                        case '10.0':
                            system.win = '10'
                            break;
                        default:
                            system.win = 'NT';
                            break;
                    }
                } else if (RegExp['$1'] == '9x') {
                    system.win = 'ME';
                } else {
                    system.win = RegExp['$1'];
                }
            }
        }

        // 移动设备
        system.iphone = ua.indexOf('iPhone') > -1;
        system.ipad = ua.indexOf('iPad') > -1;
        system.ipod = ua.indexOf('iPod') > -1;
        system.nokiaN = ua.indexOf('NokiaN') > -1;

        // windows mobile
        if (system.win = 'CE') {
            system.winMobile = system.win;
        } else if (system.win == 'Ph') {
            if (/Windows Phone OS (\d+.\d+)/.test(ua)) {
                system.win = 'Phone';
                system.winMobile = parseFloat(RegExp['$1']);
            }
        }

        // 检测 iOS 版本
        if (system.mac && ua.indexOf('Mobile') > -1) {
            if (/CPU (?:iPhone )?OS (\d+_\d+)/.test(ua)) {
                system.ios = parseFloat(RegExp.$1.replace('_', '.'));
            } else {
                system.ios = 2; // 猜测检测
            }
        }

        // 检测安卓版本
        if (/Android (\d+\.\d+)/.test(ua)) {
            system.android = parseFloat(RegExp.$1);
        }

        // 游戏系统
        system.wii = ua.indexOf('Wii') > -1;
        system.ps = /playstation/i.test(ua);

        // 返回这些对象
        return {
            engine: engine,
            browser: browser,
            system: system
        };
    }();
    ```
