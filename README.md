# JS Snippets
This is a typed version of a [Twitter Thread by @madzadev](https://twitter.com/madzadev/status/1587812568814718976?s=42&t=TjgjmkTlRQsHKct2memizw)
> 23 modern JavaScript snippets you will want to bookmark (10X productivity):

1. How to hide all specified elements
   ```js
   const hide = (...el) => [...el].forEach(e => (e.style.display = 'none'));

   // Example
   hide(document.querySelectorAll('img'));
   // Hides all <img> elements on the page
   ```

2. How to check if the element has the specified class?
   ```js
   const hasClass = (el, className) => el.classList.contains(className);

   // Example
   hasClass(document.querySelector('p.special'), 'special');  // true
   ```

3. How to toggle a class for an element?
   ```js
   class toggleClass = (el, className) => el.classList.toggle(className)

   // Example
   toggleClass(document.querySelector('p.special'), 'special');
   // The paragraph will not have the class 'special' anymore
   ```

4. How to get the scroll position of the current page?
   ```js
   const getScrollPosition = (el = window) => ({
    x: el.pageXOffset !== undefined ? el.pageXOffset : el.scrollLeft,
    y: el.pageYOffset !== undefined ? el.pageYOffset : el.scrollTop
   })

   // Example
   getScrollPosition();  // will return {x: 0, y: 200}
   ```

5. How to smooth scroll to the top of the page?
   ```js
   const scrollTop = () => {
    const c = document.documentElement.scrollTop || document.body.scrollTop;
    if (c > 0) {
      window.requestAnimationFrame(scrollToTop);
      window.scrollTo(0, c - c / 8);
    }
   };

   // Example
   scrollToTop();
   ```

6. How to check if the parent element contains the child element?
   ```js
   const elementContains = () => (parent, child) => parent !== child && parent.contains(child)

   // Example
   elementContains(document.querySelector('head')), document.querySelector('title')  // true

   elementContains(document.querySelector('body')), document.querySelector('body')  // false

   scrollToTop();
   ```

7. How to check if the element specified is visible in the viewport?
   ```js
   const elementIsVisibleInViewport (el, partiallyCisible = false) => {
    const { top, left, bottom, right } = el. getBoundingClientRect();
    const { innerHeight, innerwidth } = window;
    return partiallyVisible 
      ? ((top > 0 && top < innerHeight) || (bottom > 0 && bottom < innerHeight)) && 
        ((left > 0 && left < innerwidth) || (right > 0 && right < innerWidth))
      : top >= 0 && left > 0 && bottom <= innerHeight && right < innerwidth;
   };
   // Examples
   elementIsVisibleInViewport(el);  // (not fully visible)
   elementIsVisibleInViewport(el, true);  // (partially visible)
   ```

8. How to fetch all images within an element?
   ```js
   const getImages = (el, includeDuplicates = false) => {
     const images = [...el.getElementsByTagName('img')].map(img => img.getAttribute('src'));
     return includeDuplicates ? images : [...new Set (images)];
   };  
   // Examples
   getImages (document, true);
   // ['image1.jpg', 'Image2.png', 'image1.png', '...']
   getImages (document, false);
   // ['image1.jpg', 'image2.png', '...']
   ```

9. How to figure out if the device is a mobile device or a desktop/laptop?
   ```js
   const detectDeviceType = () =›
     /Android|webos|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)
       ? 'Mobile'
       : 'Desktop';
   
   // Example
   detectDeviceType(); // Wiil return "Mobile" or "Desktop"
   ```

10. How to get the current URL?
    ```js
    const currentURL = () => window.location.href;

    // Example
    currentUrl(); // 'https://google.com'
    ```

11. How to create an object containing the parameters of the current URL?
    ```js
    const getURLParameters = url =›
      (url.match(/([^?=&]+)(=([^&]*))/g) || [].reduce((a, v) => ((a[v.slice(0, v.indexOf ('='))] = v.slice(v.indexOf('=') + 1)), a),
      {}
      );
    
    // Examples
    getURLParameters('http://url.com/page?n=Adam&s=Smith').  // returns {n: 'Adam', s: 'Smith'}
    getURLParameters ('google.com');  // returns ()
    ```

12. How to encode a set of form elements as an object?
    ```js
    const formToobject = form =›
      Array.from(new FormData(form)).reduce(
        (acc, [key, value]) => ({
          ...acc,
          [key]: value
        }),
        {}
      );

    // Example
    formToObject(document.querySelector('#form'));
    // { email: 'test@email.com', name: 'Test Name' }
    ```

13. How to retrieve a set of properties indicated by the given selectors from an object?
    ```js
    const get = (from, ...selectors) =>
      [...selectors].map(s =>
        s
          .replace(/\[([^\[\]]*)\]/g, '.$1.')
          .split('.')
          .filter(t => t !== '')
          .reduce((prev, cur) => prev && prev[cur], from)
      );
    const obj = { selector: 
                  { to:
                    { val: 'val to select' }
                  },
                  target: [1, 2, { a: 'test' }]
                };
    // Example
    get (obj, 'selector.to.val', 'target [0]', 'target [2].a');  // ['val to select', 1, 'test']
    ```

14. How to trigger a specific event on a given element, optionally passing custom data?
    ```js
    const triggerEvent = (el, eventType, detail) =>
      el.dispatchEvent(new CustomEvent(eventType, { detail }));
    
    // Examples
    triggerEvent(document.getElementById('myId'), 'click');
    triggerEvent(document.getElementById('myId'), 'click', { username: 'bob' }); 'selector.to.val', 'target[0]', 'target[2].a');
    // ['val to select', 1, 'test']
    ```

15. How to remove an event listener from an element?
    ```js
    const off = (el, evt, fn, opts = false) =>
    el.removeEventListener(evt, fn, opts);
    
    const fn = () => console.log('!');
    document.body.addEventListener('click', fn);
    off (document.body, 'click', fn);
    // no longer logs '!' upon clicking on the page
    ```

16. How to get a readable format of the given number of milliseconds?
    ```js
    const formatDuration = ms => {
      if (ms < 0) ms = -ms;
      const time = {
        day: Math.floor(ms / 86400000),
        hour: Math.floor(ms / 3600000) % 24,
        minute: Math.floor(ms / 60000) % 60,
        second: Math.floor(ms / 1000) % 60,
        millisecond: Math.floor(ms) % 1000
      };
      return Object.entries(time)
        .filter (val => val[1] !== 0)
        .map(([key, val]) => `${val} ${key}${val !== 1 ? 's' : ''}`)
        .join(', ');
    };

    // Examples
    formatDuration(1001);  // '1 second, 1 millisecond'
    formatDuration(34325055574);  // '397 days, 6 hours, 44 minutes, 15 seconds, 574 milliseconds
    ```

17.  How to get the difference (in days) between two dates?
    ```js
    const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
      (dateFinal - dateInitial) / (1000 * 3600 * 24);
    
    // Example
    getDaysDiffBetweenDates(new Date('2017-12-13'), new Date('2017-12-22')); // 9
    ```

18. How to make a GET request to the passed URL?
    ```js
    const httpGet = (url, callback, err = console.error) => {
      const request = new XMLHttpRequest();
      request.open('GET', url, true);
      request.onload = () => callback(request.responseText);
      request.onerror = () => err(request);
      request.send();
    };
    
    httpGet('https://jsonplaceholder.typicode.com/posts/1', console.log);
    // Logs: {"userId": 1, "id": 1, "title": "sample title", "body": "my text"}
    ```

19. How to make a POST request to the passed URL? 
    ```js
    const httpPost = (url, data, callback, err = console.error) => {
      const request = new XMLHttpRequest();
      request.open('POST', url, true);
      request.setRequestHeader('Content-type', 'application/json; charset=utf-8');
      request.onload = () => callback(request.responseText);
      request.onerror = () => err(request);
      request.send(data);
    };

    const newPost a {
      userId: 1,
      id: 1337,
      title: 'Foo',
      body: 'bar bar bar'
    };

    const data = JSON.stringify(newPost);
    httpPost('https://jsonplaceholder.typicode.com/posts', data, console.log);
    // Logs: {"userId": 1, "Id": 1337, "title": "Foo", "body": "bar bar bar"}
    ```

20. How to create a counter with the specified range, step, and duration for the specified selector?
    ```js
    const counter = (selector, start, end, step = 1, duration = 2000) => {
      let current = start,
        _step = (end - start) * step < 0 ? -step : step,
        timer = setinterval(0) => {
          current += _step;
          document.querySelector(selector).innerHTML = current;
          if (current >= end) document.querySelector(selector).innerHTML = end;
          if (current >= end) clearInterval(timer);
        },
        Math.abs(Math.floor(duration / (end - start))));
      return timer;
    };

    // Example
    counter ('#my-id', 1, 1000, 5, 2000)
    // Creates a 2-second timer for the element with id="my-id"
    ```

21. How to copy a string to the clipboard?
    ```js
    const copyToClipboard = str => {
      const el = document.createElement('textarea');
      el.value = str;
      el.setAttribute('readonly', '');
      el.style.position = 'absolute';
      el.style.left = '-9999px';
      document.body.appendChild(el);
      const selected =
        document.getSelection().rangeCount > 0 ? document.getSelection().getRangeAt(0) : false;
      el.select();
      document.execCommand('copy');
      document.body.removeChild(el);
      If (selected) {
        document.getSelection().removeAllRanges();
        document.getSelection().addRange(selected);
      }
    };

    // Example
    copyToClipboard('Lorem ipsum');  // 'Lorem ipsum' copied to clipboard.
    ```

22. How to find out if the browser tab of the page is focused?
    ```js
    const isBrowserTabFocused = () => !document.hidden;

    // Example
    isBrowserTabFocused();  // true
    ```

23. How to create a directory, if it does not exist?
    ```js
    const fs = require('fs');
    const createDirIfNotExists = dir => (!fs.existsSync(dir) ? fs.mkdirSync(dir) : undefined);

    // Example
    createDirIfNotExists('test');  // Creates the directory 'test', if it doesn't exist
    ```
