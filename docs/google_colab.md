# Google Colab



Google Colabでインストールされているパッケージの確認

```python
import pip
installed_packages = pip.get_installed_distributions()
installed_packages_list = sorted(["%s==%s" % (i.key, i.version)
for i in installed_packages])
print(installed_packages_list)
```



driveをマウント

```python
from google.colab import drive
drive.mount('/content/drive')
```



```python
%pwd

# >> '/content'
```



```python
cd drive/My\ Drive/Colab_Notebooks
```





CPUスペックの確認

```python
!cat /proc/cpuinfo
```





インストールされているライブラリの確認

```python
!pip list
```



OS確認

```python
!cat /etc/issue
```



```python
#Google Colabを起動してからの時間確認
!cat /proc/uptime | awk '{print $1 /60 /60 /24 "days (" $1 "sec)"}'
```

