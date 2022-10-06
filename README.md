# Youtube-Navigator

`alt+i` and `alt+k` to navigate up and down the video results in a youtube search results page.  
`space` or `enter` to open the currently focused video.  

### Demo
![Peek 2022-10-06 23-14](https://user-images.githubusercontent.com/68982541/194383452-22ccbedb-6dad-4c5e-b66a-153c28357787.gif)

```javascript
// ==UserScript==
// @name        Youtube Navigator
// @namespace   Violentmonkey Scripts
// @match       https://www.youtube.com/results*
// @grant       none
// @version     1.0
// @run-at      document-end
// @author      -
// @description 06/10/2022, 22:21:19
// ==/UserScript==

console.log('Youtube Navigator Script Active');

let currentIndex = -1;
let allVideoLinks;
let prevLink;
let allVideoContainers;
let searchBar;

setTimeout(()=>{
  allVideoContainers = document.querySelectorAll('#contents.style-scope.ytd-item-section-renderer > *');
  allVideoLinks = document.querySelectorAll('#title-wrapper h3 a#video-title');
  searchBar = document.querySelector('input');
}, 500);

document.onkeydown = e=>{
  console.log(e.key, 'KEY')
  if(e.altKey && e.key === 'k'){
    ++currentIndex;
    if(prevLink?.style?.border){
      prevLink.style.border = 'none';
    }
    allVideoContainers[currentIndex].style.border = '1px solid #689d6a';
    allVideoContainers[currentIndex].style.borderRadius = '6px';
    allVideoContainers[currentIndex].style.padding = '2px 4px';
    allVideoLinks[currentIndex].focus();
    prevLink = allVideoContainers[currentIndex];
  }
  
  if(e.altKey && e.key === 'i'){
    if(currentIndex < 0){
      return;
    }
    --currentIndex;
    if(prevLink?.style?.border){
      prevLink.style.border = 'none';
    }
    allVideoContainers[currentIndex].style.border = '1px solid #689d6a';
    allVideoContainers[currentIndex].style.borderRadius = '6px';
    allVideoContainers[currentIndex].style.padding = '2px 4px';
    allVideoLinks[currentIndex].focus();
    prevLink = allVideoContainers[currentIndex];
  }
  
  if(e.key == " " ||
      e.code == "Space" ||      
      e.keyCode == 32  ){
    // return if serach bar is active and user is pressing space bar
    if(document.activeElement === searchBar){
      return;
    }
    allVideoLinks[currentIndex].focus();
    document.activeElement.click();
  }
}
```
