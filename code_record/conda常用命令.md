### conda常用命令

- 查看conda版本

  ```shell
  conda -V
  ```

- conda常用

  ```shell
  conda list #查看安装了那些包（会显示当前环境下）
  conda env list 或 conda info -e #查看当前存在哪些虚拟环境
  conda updata conda #检查更新当前版本
  conda --version #查询conda版本
  conda -h #查询conda的命令使用、
  conda config --show #查看配置，包括channles
  conda config --show channels #查看channels
  conda config --remove channels channel_Url #删除一个channels
  conda config --add channels channel_Url #增添一个channels
  python -m pip install --upgrade pip #更新pip
  ```

- 创建虚拟环境

  ```shell
  conda create -n your_env_name python=X.X
  ```

- 使用激活虚拟环境

  ```shell
  activate your_env_name #激活虚拟环境
  conda install -n your_env_name pakage_name #对虚拟环境安装所需的包
  conda deactivate #关闭虚拟环境
  conda remove -n your_env_name --all #删除虚拟环境
  conda remove --name your_env_name package_name #删除虚拟环境中的某个包
  conda install python==3.8 -n "指定虚拟环境的名字" #修改虚拟环境python版本
  
  ```


- 镜像修改

  ```shell
  1,2两行代码连接清华镜像
  1. conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ 
  2.conda config --set show_channel_urls yes
  
  ```

  