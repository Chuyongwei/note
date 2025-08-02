# 总结

## 基本操作

```sh
conda create -n xxx python=3.9
conda env list
conda info --envs

conda remove --name 环境名 --all
```



```sh
conda install mingw libpython
python -m pip install --upgrade pip
```

## 清理缓存

```bash
# 删除没有用的包
conda clean -p
# 删除tar打包
conda clean -t
# 删除无用的包和缓存
conda clean --all
```

