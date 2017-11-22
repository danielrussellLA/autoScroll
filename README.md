# autoScroll
The autoScroll.js file contains all the code you need to automatically scroll to an element with vanilla javascript. It works well across all devices.

invoking it looks like:
```
autoScroll('.some-className');
```

The complete snippet in autoScroll.js contains the autoScroll function and a vanilla debounce function. 
```
function debounce(func, wait, immediate) {
    var timeout;
    return function() {
        var context = this, args = arguments;
        var later = function() {
            timeout = null;
            if (!immediate) func.apply(context, args);
        };
        var callNow = immediate && !timeout;
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
        if (callNow) func.apply(context, args);
    };
};

function autoScroll(selector) {
    var scrollAttempts = 0;
    var incrementScrollAttempts = debounce(function() {
        scrollAttempts++;
    });

    window.addEventListener('scroll', incrementScrollAttempts);

    var el = document.querySelector(selector);
    var chkReadyState = setInterval(function() {
        if (el) {
            window.scrollTo(0, el.offsetTop);
        }
        if (document.readyState == 'complete' || scrollAttempts > 1) {
            clearInterval(chkReadyState);
            window.removeEventListener('scroll', incrementScrollAttempts, false);
        }
    }, 100);
};
```
