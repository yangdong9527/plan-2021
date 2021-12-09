使用`LoadingManager`监控加载过程

```js
import { gsap } from 'gsap'
const loadingManager = new THREE.LoadingManager(
	() => {
    // loaded
    // 小优化 可能碰到滚动条动画没结束就直接隐藏滚动条
    //setTimeout(() => { //todo }, 500)
    
    gsap.to(overlayMaterial.uniforms.uAlpha, { duration: 3, value: 0 })
  },
  (itemUrl, curr, total) => {
    // progress
    const progress = curr / total
  }
)
```

顺便教了一个 进度条的 制作方法