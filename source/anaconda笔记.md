# anaconda笔记

### 1.环境基本命令

#### 1.创建虚拟环境  
    ```
    conda create -n env_name（环境名称） python=3.7(对应的python版本号）  
    ```
#### 2. 激活虚拟环境  
    ```
    conda activate env_name（环境名称）  
    ```
#### 3. 退出虚拟环境  
    ```
    deactivate env_name（环境名称）
    ```
#### 4. 删除虚拟环境  
    ```
    conda remove -n env_name(环境名称) --all
    ```
#### 5. 查看安装了哪些包
    ```
    conda list 
    ```
#### 6.查看当前存在哪些虚拟环境
    ```
    conda env list 或 conda info -e或 conda info --envs
    ```
#### 7.克隆环境
    ```
    conda create -n B --clone A
    ```
#### 8.对虚拟环境中安装额外的包
    ```
    conda install -n your_env_name
    ```
#### 9.删除环境中的某个包
    ```
    conda remove --name your_env_name package_name
    ```



