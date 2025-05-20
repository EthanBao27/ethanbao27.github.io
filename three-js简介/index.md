# Three.js简介


# Three.Js 介绍

Three.js 由 Ricardo Cabello 在 2010 四月于 GitHub 首次发布。

Three.js 是一个跨浏览器的使用 JavaScript 函数库或 API 来在网页浏览器中创建和展示三维计算机图形的开源通用 3D 代码库。Three.js 使用 WebGL 渲染图形，也可通过插件使用 WebGPU（实验性）、SVG 和 CSS3D 渲染器。源代码托管在 GitHub。

## Installation

每个 three.js 项目至少需要一个 HTML 文件来定义网页，以及一个 JavaScript 文件来运行你的 three.js 代码。下面的结构和命名选择并非必需，但为了保持一致性，本指南将在全文中使用。

- `index.html`

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="utf-8" />
      <title>My first three.js app</title>
      <style>
        body {
          margin: 0;
        }
      </style>
    </head>
    <body>
      <script
        type="module"
        src="/main.js"></script>
    </body>
  </html>
  ```

- `main.js`

  ```js
  import * as THREE from 'three';

  ...
  ```

- `public/`

  - _public/_ 文件夹有时也被称为 "静态（static）"文件夹，因为其中包含的文件会原封不动地推送到网站上。纹理（textures）、音频和 3D 模型通常会放在这里。

  ## Creating a Scene

  ```js
  import * as THREE from "three";

  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );

  const renderer = new THREE.WebGLRenderer();
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  const geometry = new THREE.BoxGeometry(1, 1, 1);
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);

  camera.position.z = 5;

  function animate() {
    requestAnimationFrame(animate);
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.02;
    renderer.render(scene, camera);
  }
  animate();
  ```

  - 以上这行代码创建了一个不断旋转的正方体。
  - ![效果图](/images/threejs.png)

## 解释代码

我们花一点点时间来解释一下这里发生了什么。我们现在建立了场景、相机和渲染器。

three.js 里有几种不同的相机，在这里，我们使用的是 **PerspectiveCamera**（透视摄像机）。

第一个参数是**视野角度（FOV）**。视野角度就是无论在什么时候，你所能在显示器上看到的场景的范围，它的单位是角度(与弧度区分开)。

第二个参数是**长宽比（aspect ratio）**。 也就是你用一个物体的宽除以它的高的值。比如说，当你在一个宽屏电视上播放老电影时，可以看到图像仿佛是被压扁的。

接下来的两个参数是**近截面**（near）和**远截面**（far）。 当物体某些部分比摄像机的**远截面**远或者比**近截面**近的时候，该这些部分将不会被渲染到场景中。或许现在你不用担心这个值的影响，但未来为了获得更好的渲染性能，你将可以在你的应用程序里去设置它。

接下来是渲染器。这里是施展魔法的地方。除了我们在这里用到的 WebGLRenderer 渲染器之外，Three.js 同时提供了其他几种渲染器，当用户所使用的浏览器过于老旧，或者由于其他原因不支持 WebGL 时，可以使用这几种渲染器进行降级。

除了创建一个渲染器的实例之外，我们还需要在我们的应用程序里设置一个渲染器的尺寸。比如说，我们可以使用所需要的渲染区域的宽高，来让渲染器渲染出的场景填充满我们的应用程序。因此，我们可以将渲染器宽高设置为浏览器窗口宽高。对于性能比较敏感的应用程序来说，你可以使用 **setSize** 传入一个较小的值，例如 **window.innerWidth/2** 和 **window.innerHeight/2**，这将使得应用程序在渲染时，以一半的长宽尺寸渲染场景。

如果你希望保持你的应用程序的尺寸，但是以较低的分辨率来渲染，你可以在调用 **setSize** 时，将 **updateStyle**（第三个参数）设为 false。例如，假设你的 canvas 标签现在已经具有了 100% 的宽和高，调用 **setSize(window.innerWidth/2, window.innerHeight/2, false)** 将使得你的应用程序以四分之一的大小来进行渲染。

最后一步很重要，我们将 **renderer**（渲染器）的 dom 元素（renderer.domElement）添加到我们的 HTML 文档中。这就是渲染器用来显示场景给我们看的 canvas 元素。

要创建一个立方体，我们需要一个 **BoxGeometry**（立方体）对象. 这个对象包含了一个立方体中所有的顶点（**vertices**）和面（**faces**）。未来我们将在这方面进行更多的探索。

接下来，对于这个立方体，我们需要给它一个材质，来让它有颜色。Three.js 自带了几种材质，在这里我们使用的是 **MeshBasicMaterial**。所有的材质都存有应用于他们的属性的对象。在这里为了简单起见，我们只设置一个 color 属性，值为 **0x00ff00**，也就是绿色。这里所做的事情，和在 CSS 或者 Photoshop 中使用十六进制（**hex colors**）颜色格式来设置颜色的方式一致。

第三步，我们需要一个 **Mesh**（网格）。 网格包含一个几何体以及作用在此几何体上的材质，我们可以直接将网格对象放入到我们的场景中，并让它在场景中自由移动。

默认情况下，当我们调用 **scene.add()** 的时候，物体将会被添加到 **(0,0,0)** 坐标。但将使得摄像机和立方体彼此在一起。为了防止这种情况的发生，我们只需要将摄像机稍微向外移动一些即可。

## 渲染场景

现在，如果将之前写好的代码复制到 HTML 文件中，你不会在页面中看到任何东西。这是因为我们还没有对它进行真正的渲染。为此，我们需要使用一个被叫做“**渲染循环**”（render loop）或者“**动画循环**”（animate loop）的东西。

```
function animate() { requestAnimationFrame( animate ); renderer.render( scene, camera ); } animate();
```

在这里我们创建了一个使渲染器能够在每次屏幕刷新时对场景进行绘制的循环（在大多数屏幕上，刷新率一般是 60 次/秒）。如果你是一个浏览器游戏开发的新手，你或许会说*“为什么我们不直接用 setInterval 来实现刷新的功能呢？”*当然啦，我们的确可以用 setInterval，但是，**requestAnimationFrame** 有很多的优点。最重要的一点或许就是当用户切换到其它的标签页时，它会暂停，因此不会浪费用户宝贵的处理器资源，也不会损耗电池的使用寿命。

## 使立方体动起来

在开始之前，如果你已经将上面的代码写入到了你所创建的文件中，你可以看到一个绿色的方块。让我们来做一些更加有趣的事 —— 让它旋转起来。

将下列代码添加到 animate() 函数中 **renderer.render** 调用的上方：

```
cube.rotation.x += 0.01; cube.rotation.y += 0.01;
```

这段代码每帧都会执行（正常情况下是 60 次/秒），这就让立方体有了一个看起来很不错的旋转动画。基本上来说，当应用程序运行时，如果你想要移动或者改变任何场景中的东西，都必须要经过这个动画循环。当然，你可以在这个动画循环里调用别的函数，这样你就不会写出有上百行代码的 **animate** 函数。

## 结果

祝贺你！你现在已经成功完成了你的第一个 three.js 应用程序。虽然它很简单，但现在你已经有了一个入门的起点。

