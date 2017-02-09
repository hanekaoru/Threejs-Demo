## 一个简单的示例

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style>canvas { width: 100%; height: 100% }</style>
</head>
<body>
    <script src="https://cdn.staticfile.org/three.js/r83/three.js"></script>
    <script>
        var scene = new THREE.Scene();
        
        var camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
        
        var renderer = new THREE.WebGLRenderer();
        
        renderer.setSize(400, 400);
        
        document.body.appendChild(renderer.domElement);
        var geometry = new THREE.CubeGeometry(1,1,1);
        var material = new THREE.MeshBasicMaterial({color: 0x00ff00});
        var cube = new THREE.Mesh(geometry, material); scene.add(cube);
        camera.position.z = 5;
        function render() {
            requestAnimationFrame(render);
            cube.rotation.x += 0.1;
            cube.rotation.y += 0.1;
            renderer.render(scene, camera);
        }
        render();
    </script>

</body>
</html>
```


## 1. 三大组件

在 ```Three.js``` 中，要渲染物体到网页中，我们需要 3 个组件：场景（scene）、相机（camera）和渲染器（renderer）。有了这**三样东西**，才能将物体渲染到网页中去

三要素的代码如下：

```js
var scene = new THREE.Scene();  // 场景
var camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);// 透视相机
var renderer = new THREE.WebGLRenderer();   // 渲染器

renderer.setSize(window.innerWidth, window.innerHeight);    // 设置渲染器的大小为窗口的内宽度，也就是内容区的宽度
document.body.appendChild(renderer.domElement);
```

在 Threejs 中场景只有一种，用 ```THREE.Scene``` 来表示，要构建一个场景也很简单，只需要 ```new``` 一个对象就可以了，代码如下：

```js
var scene = new THREE.Scene();
```

场景是所有物体的容器，如果要显示一个苹果，就需要将苹果对象加入场景中


