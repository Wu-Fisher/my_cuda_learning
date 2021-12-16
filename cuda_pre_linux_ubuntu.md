# cuda_pre_linux_ubuntu(apt)
## 1.download
    https://developer.nvidia.com/cuda-downloads
    and select your platform to get the instructions to install it
```bash
(update 12.9)
$ wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
$ sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
$ wget https://developer.download.nvidia.com/compute/cuda/11.5.1/local_installers/cuda-repo-ubuntu2004-11-5-local_11.5.1-495.29.05-1_amd64.deb
$ sudo dpkg -i cuda-repo-ubuntu2004-11-5-local_11.5.1-495.29.05-1_amd64.deb
$ sudo apt-key add /var/cuda-repo-ubuntu2004-11-5-local/7fa2af80.pub
$ sudo apt-get update
$ sudo apt-get -y install cuda
```
then reboot

and add these below in bashrc 
```vim
$ export PATH=/usr/local/cuda-11.5/bin${PATH:+:${PATH}}
$ export LD_LIBRARY_PATH=/usr/local/cuda-11.5/lib64\${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

maybe you need some lib(not all)
```bash
$ sudo apt-get install build-essential libgl1-mesa-dev
$ sudo apt-get install freeglut3-dev
$ sudo apt-get install libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev


```

## well make a test
### 1
```bash
$ cuda-install-samples-11.5.sh ~
$ cd ~/NVIDIA_CUDA-11.5_Samples/5_Simulations/nbody
$ make
$ ./nbody
```
### 2
```c
// test.c
/* light.c
此程序利用GLUT绘制一个OpenGL窗口，并显示一个加以光照的球。
*/
/* 由于头文件glut.h中已经包含了头文件gl.h和glu.h，所以只需要include 此文件*/
# include <GL/glut.h>
# include <stdlib.h>
    
/* 初始化材料属性、光源属性、光照模型，打开深度缓冲区 */
void init ( void )
{
    GLfloat mat_specular [ ] = { 1.0, 1.0, 1.0, 1.0 };
    GLfloat mat_shininess [ ] = { 50.0 };
    GLfloat light_position [ ] = { 1.0, 1.0, 1.0, 0.0 };
    glClearColor ( 0.0, 0.0, 0.0, 0.0 );
    glShadeModel ( GL_SMOOTH );
    glMaterialfv ( GL_FRONT, GL_SPECULAR, mat_specular);
    glMaterialfv ( GL_FRONT, GL_SHININESS, mat_shininess);
    glLightfv ( GL_LIGHT0, GL_POSITION, light_position);
    glEnable (GL_LIGHTING);
    glEnable (GL_LIGHT0);
    glEnable (GL_DEPTH_TEST);
}
/*调用GLUT函数，绘制一个球*/
void display ( void )
{
    glClear (GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glutSolidSphere (1.0, 40, 50);
    glFlush ();
}
    
int main(int argc, char** argv)
{
    /* GLUT环境初始化*/
    glutInit (&argc, argv);
    /* 显示模式初始化 */
    glutInitDisplayMode (GLUT_SINGLE | GLUT_RGB | GLUT_DEPTH);
    /* 定义窗口大小 */
    glutInitWindowSize (300, 300);
    /* 定义窗口位置 */
    glutInitWindowPosition (100, 100);
    /* 显示窗口，窗口标题为执行函数名 */
    glutCreateWindow ( argv [ 0 ] );
    /* 调用OpenGL初始化函数 */
    init ( );
    /* 注册OpenGL绘图函数 */
    glutDisplayFunc ( display );
    // /* 进入GLUT消息循环，开始执行程序 */
    glutMainLoop( );
    return 0;
} 
```
```bash
$ cc test.c -o test -lGL -lglut
$ ./test
```
## 编译
    nvcc会自动链接cuda库
    而gcc不会，如果需要需要自己链接一下
## tips

### 编译时警告vncc warning: The ‘compute_35’, ‘compute_37’, ‘compute_50’, ‘sm_35’, ‘sm_37’ and ‘sm_50’ architectures are deprecated, and may be removed in a future release
    cuda11已经弃用这几种计算    