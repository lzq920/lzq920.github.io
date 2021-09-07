---
title: 原生App内部嵌入WebViewJavascriptBridge
---

原生App嵌入WebViewJavascriptBridge协议，实现前端调用原生系统功能

> 注册协议 

```
(function () {
    const isiOS = navigator.userAgent.toLowerCase().includes('iOS')
    const isAndroid = navigator.userAgent.toLowerCase().includes('Android')
    /**
     * Android  与安卓交互时：
     *      1、不调用这个函数安卓无法调用 H5 注册的事件函数；
     *      2、但是 H5 可以正常调用安卓注册的事件函数；
     *      3、还必须在 setupWebViewJavascriptBridge 中执行 bridge.init 方法，否则：
     *          ①、安卓依然无法调用 H5 注册的事件函数
     *          ①、H5 正常调用安卓事件函数后的回调函数无法正常执行
     *
     * @param {*} callback
     */
    const androidFunction = (callback) => {
        if (window.WebViewJavascriptBridge) {
            callback(window.WebViewJavascriptBridge)
        } else {
            document.addEventListener('WebViewJavascriptBridgeReady', function () {
                callback(window.WebViewJavascriptBridge)
            }, false)
        }
    }
    /**
     * IOS 与 IOS 交互时，使用这个函数即可，别的操作都不需要执行
     * @param {*} callback
     */
    const iosFunction = (callback) => {
        if (window.WebViewJavascriptBridge) {
            return callback(window.WebViewJavascriptBridge)
        }
        if (window.WVJBCallbacks) {
            return window.WVJBCallbacks.push(callback)
        }
        window.WVJBCallbacks = [callback]
        const WVJBIframe = document.createElement('iframe')
        WVJBIframe.style.display = 'none'
        WVJBIframe.src = 'wvjbscheme://__BRIDGE_LOADED__'
        document.documentElement.appendChild(WVJBIframe)
        setTimeout(function () {
            document.documentElement.removeChild(WVJBIframe)
        }, 0)
    }
    /**
     * 注册 setupWebViewJavascriptBridge 方法
     *  之所以不将上面两个方法融合成一个方法，是因为放在一起，那么就只有 iosFunction 中相关的方法体生效
     */
    window.setupWebViewJavascriptBridge = isAndroid ? androidFunction : iosFunction
    /**
     * 这里如果不做判断是不是安卓，而是直接就执行下面的方法，就会导致
     *      1、IOS 无法调用 H5 这边注册的事件函数
     *      2、H5 可以正常调用 IOS 这边的事件函数，并且 H5 的回调函数可以正常执行
     */
    if (isAndroid) {
        /**
         * 与安卓交互时，不调用这个函数会导致：
         *      1、H5 可以正常调用 安卓这边的事件函数，但是无法再调用到 H5 的回调函数
         *
         * 前提 setupWebViewJavascriptBridge 这个函数使用的是 androidFunction 这个，否则还是会导致上面 1 的现象出现
         */
        window.setupWebViewJavascriptBridge(function (bridge) {
            // 注册 H5 界面的默认接收函数（与安卓交互时，不注册这个事件无法接收回调函数）
            bridge.init(function (msg, responseCallback) {
                responseCallback('JS 返回给原生的消息内容')
            })
        })
    }
})()
```

> 注册事件

```

export default function useBridge () {
  const openLogin = () => {
    return new Promise((resolve, reject) => {
      window.setupWebViewJavascriptBridge(bridge => {
        bridge.callHandler('login', (data) => {
          resolve()
        })
      })
    })
  }
  const getClientHeader = () => {
    return new Promise((resolve, reject) => {
      window.setupWebViewJavascriptBridge(bridge => {
        bridge.callHandler('getheaderinfo', (data) => {
          resolve(data)
        })
      })
    })
  }
  const goBack = () => {
    return new Promise((resolve, reject) => {
      window.setupWebViewJavascriptBridge(bridge => {
        bridge.callHandler('goback', (data) => {
          resolve()
        })
      })
    })
  }
  return {
    openLogin,
    goBack,
    getClientHeader
  }
}

```

