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
        var cube = new THREE.Mesh(geometry, material); 

        scene.add(cube);
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


## 2. 相机

相机决定了场景中那个角度的景色会显示出来。相机就像人的眼睛一样，人站在不同位置，抬头或者低头都能够看到不同的景色

在 ```Threejs``` 中有多种相机，这里介绍两种，它们是：

* 正投影相机 ```THREE.OrthographicCamera``` 

* 透视投影相机 ```THREE.PerspectiveCamera```

定义方式如下：（具体参数后面会涉及到）

```js
// 透视相机（THREE.PerspectiveCamera）
var camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
```


## 3. 渲染器

渲染器决定了渲染的结果应该画在页面的什么元素上面，并且以怎样的方式来绘制，定义方式如下：

```js
var renderer = new THREE.WebGLRenderer();

renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```

注意，渲染器 ```renderer``` 的 ```domElement``` 元素，表示渲染器中的画布，所有的渲染都是画在 ```domElement``` 上的，所以这里的 ```appendChild``` 表示将这个 ```domElement``` 挂接在 ```body``` 下面，这样渲染的结果就能够在页面中显示了



## 4. 添加物体到场景中

```js
var geometry = new THREE.CubeGeometry(1, 1, 1);
var material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });

var cube = new THREE.Mesh(geometry, material);
scene.add(cube);
```

CubeGeometry 表示的是一个正方体或者长方体，由它的 3 个参数所决定，原型如下：

```js
CubeGeometry(width, height, depth, segmentsWidth, segmentsHeight, segmentsDepth, materials, sides)
```

* width： 立方体 x 轴的长度

* height： 立方体 y 轴的长度

* depth： 立方体 z 轴的深度，也就是长度



## 5. 渲染

渲染应该使用渲染器，结合前面的相机和场景来得到结果画面，实现这个功能的函数是：

```js
renderer.render(scene, camera);
```

渲染函数的原型如下：

```js
render(scene, camera. renderTarget, forceClear)
```

* scene： 场景

* camera：  相机

* renderTarget： 渲染的目标，默认是渲染到前面定义的 render 变量中

* forceClear： 每次绘制之前都将画布的内容给清除，即使自动清除标志 autoClear 为 false，也会清除



## 6. 渲染循环

渲染有两种方式：实时渲染 和 离线渲染

* 离线渲染： 好比如电影中的 CG 场景，都是事先先渲染好一帧一帧的图片，然后在拼接成电影

* 实时渲染： 需要不停的对画面进行渲染，即使画面中什么也没有改变，也需要重新渲染

一个渲染循环的实现：

```js
function render() {
    cube.rotation.x += 0.1;
    cube.rotation.y += 0.1;
    renderer.render(scene, camera);
    requestAnimationFrame(render);
}
```

其中一个重要的函数是 ```requestAnimationFrame```，这个函数就是让浏览器去执行一次参数中的函数，这样通过上面 ```render``` 中调用 ```requestAnimationFrame()``` 函数，```requestAnimationFrame()``` 函数又让 ```rander()``` 再执行一次，就形成了我们通常所说的游戏循环了



## 场景，相机，渲染器之间的关系

场景就是一个物体的容器，相机的作用就是面对场景，在场景中取一个合适的景，把它拍下来，渲染器的作用就是将相机拍摄下来的图片，放到浏览中去显示

![场景，相机，渲染器之间的关系](http://www.hewebgl.com/attached/image/20130810/20130810150021_257.jpg)



## 重构

上面代码是在一段脚本中完成，当逻辑复杂后，就不容易理解，现在按照功能分解成函数

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Three框架</title>
        <script src="https://cdn.staticfile.org/three.js/r83/three.js"></script>
        <style type="text/css">
            div#canvas-frame {
                border: none;
                cursor: pointer;
                width: 100%;
                height: 600px;
                background-color: #EEEEEE;
            }

        </style>
        <script>

            // 渲染器
            var renderer;
            function initThree() {
                width = document.getElementById('canvas-frame').clientWidth;
                height = document.getElementById('canvas-frame').clientHeight;
                renderer = new THREE.WebGLRenderer({
                    antialias: true
                });
                renderer.setSize(width, height);
                document.getElementById('canvas-frame').appendChild(renderer.domElement);
                renderer.setClearColor(0xFFFFFF, 1.0);
            }

            // 相机
            var camera;
            function initCamera() {
                camera = new THREE.PerspectiveCamera(45, width / height, 1, 10000);
                camera.position.x = 0;
                camera.position.y = 1000;
                camera.position.z = 0;
                camera.up.x = 0;
                camera.up.y = 0;
                camera.up.z = 1;
                camera.lookAt({
                    x: 0,
                    y: 0,
                    z: 0
                });
            }

            // 场景
            var scene;
            function initScene() {
                scene = new THREE.Scene();
            }

            // 添加物体到场景中
            var light;
            function initLight() {
                light = new THREE.DirectionalLight(0xFF0000, 1.0, 0);
                light.position.set(100, 100, 200);
                scene.add(light);
            }

            var cube;
            function initObject() {

                var geometry = new THREE.Geometry();
                var material = new THREE.LineBasicMaterial({ vertexColors: THREE.VertexColors });
                var color1 = new THREE.Color(0x444444), color2 = new THREE.Color(0xFF0000);

                // 线的材质可以由2点的颜色决定
                var p1 = new THREE.Vector3(-100, 0, 100);
                var p2 = new THREE.Vector3(100, 0, -100);
                geometry.vertices.push(p1);
                geometry.vertices.push(p2);
                geometry.colors.push(color1, color2);

                var line = new THREE.Line(geometry, material, THREE.LinePieces);
                scene.add(line);
            }

            // 循环渲染
            function render() {
                renderer.clear();
                renderer.render(scene, camera);
                requestAnimationFrame(render);
            }

            // 初始化
            function threeStart() {
                initThree();
                initCamera();
                initScene();
                initLight();
                initObject();
                render();
            }

        </script>
    </head>

    <body onload="threeStart();">
        <div id="canvas-frame"></div>
    </body>
</html>
```
