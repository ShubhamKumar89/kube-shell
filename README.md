# kube-shell

Kube-shell is an integrated shell for working with the Kubernetes CLI. <br>
Provide Auto Completion of Commands and Options with in-line documentation. [Github](https://github.com/cloudnativelabs/kube-shell)

### Installation
```
sudo apt install python3-pip
pip install kube-shell
```
```
export PATH=$PATH:~/.local/bin
source ~/.bashrc
```

### Start

```
kube-shell
```
#### Known ERROR:

```
/Applications/anaconda3/lib/python3.7/site-packages/kubernetes/config/kube_config.py:280: YAMLLoadWarning: calling yaml.load() without Loader=... is deprecated, as the default Loader is unsafe. Please read https://msg.pyyaml.org/load for full details.
  config_dict=yaml.load(f),
/Applications/anaconda3/lib/python3.7/site-packages/kubeshell/kubeshell.py:43: YAMLLoadWarning: calling yaml.load_all() without Loader=... is deprecated, as the default Loader is unsafe. Please read https://msg.pyyaml.org/load for full details.
  for doc in docs:
  ```
#### SOLUTION:

To resolve the warning messages, you can modify the code to specify the Loader parameter  with the FullLoader implementation while calling the yaml.load() and yaml.load_all() functions.

in **kubernetes/config/kube_config.py** file,
modify the `config_dict = yaml.load(f)` with `config_dict = yaml.load(f, Loader=yaml.FullLoader)`

and in **kubeshell/kubeshell.py** file,
modify the `for doc in docs:` with `for doc in yaml.load_all(docs, Loader=yaml.FullLoader):`

The location of the **kubernetes/config** and **kubeshell/** directory depends on how you installed the Kubernetes client library. If you installed the library using pip, the directory is typically located in the site-packages directory of your Python installation.

To find the location, you can use the following command in a terminal:
```
pip show kubernetes | grep Location
## Location: /usr/local/lib/python3.8/dist-packages
```

To Change: 

```
sed  -i "s/config_dict = yaml.load(f)/config_dict = yaml.load(f, Loader=yaml.FullLoader)/g" /usr/local/lib/python3.8/dist-packages/kubernetes/config/kube_config.py

sed -i "s/for doc in docs:/for doc in yaml.load_all(docs, Loader=yaml.FullLoader):/g" /usr/local/lib/python3.8/dist-packages/kubeshell/kubeshell.py
```

***Note: Change the location with yours.***
