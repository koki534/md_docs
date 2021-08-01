# Anacoda コマンド
## 基本
`conda update conda`でcondaのアップデート

`conda create -n 仮想環境名 python=バージョン番号` 仮想環境作成

`conda list` パッケージリスト表示

`conda env list` 仮想環境リスト表示

`conda remove -n 仮想環境名 --all` 仮想環境を削除

`conda activate 仮想環境名` 仮想環境に入る

`conda deactivate` 仮想環境を抜ける

`conda search パッケージ名` パッケージがcondaでインストールできるか確認

`conda install パッケージ名==バージョン番号` パッケージをインストール

requirements.txtからcondaでインストール

```bash
conda install --file requirements.txt
```



`conda deactivate` 仮想環境を抜ける

## jupyter lab関連(新)

1. 仮想環境の外でnb_conda_kernelsのインストール
`conda install jupycon/label/dev::nb_conda_kernels`

2. 仮想環境を作成してactivateする
`conda create -n 仮想環境名 python=バージョン番号`

3. 仮想環境内でipykernelをインストール
`conda install ipykernel`

これでjupyterlabのLauncherから簡単に仮想環境を切り替えることができるようになる

<https://dwflanagan.github.io/2018-04-20-conda-envs-in-jupyter/>

## jupyter lab関連(旧)

`ipython kernel install --user --name=仮想環境名` 新しいカーネルをJupyterに追加

`jupyter kernelspec list` カーネル(仮想環境)のリストを返す

`jupyter kernelspec uninstall 仮想環境名` カーネルの表示を削除

参考
https://anbasile.github.io/posts/2017-06-25-jupyter-venv/

https://janakiev.com/blog/jupyter-virtual-envs/