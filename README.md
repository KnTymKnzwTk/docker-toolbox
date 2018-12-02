# docker-toolbox環境構築
Windows 10 Home Edition でDocker入れた時のメモ



1. Docker Toolboxをダウンロードする

   https://docs.docker.com/toolbox/toolbox_install_windows/からダウンロード

2. インストーラを実行

   Select Components : Git入ってない場合は [ Git for Windows ] にチェック。

   → 直接必要な訳ではないけど、どのみち必要になるはずだから。

   Select Additional Tasks: [ Install VirtualBox with NDIS5 driver[default NDIS6 ] にチェック。

   → 理由は分からないけどチェック入れずに進めるとDocker用の仮想マシン`default`が起動できなかった。

3. 仮想マシンの設定

   Virtualboxを起動し、仮想マシンの一覧から`default`という名前を探す。選択した状態で上部メニューの「設定」を選択。
   「共有フォルダー」を選択し、Dockerで使用したい任意のフォルダを共有フォルダとして登録しておく。
   → Docker ToolboxはDocker for Mac/Windowsと違ってVirtualbox上の仮想マシンで動いているため、ホストOSのフォルダを直接参照できない。
     この設定を忘れてるとvolumesのマウントなどで詰むハメになるので注意。
     Windowsの場合 `¥¥C:¥Users¥` 直下は自動マウントされるという情報もあったが未確認。
   また、「ネットワーク」を選択し、ホストOSとゲストOSで通信が可能なようにネットワークを設定する。
   ローカルサーバの確認だけであればホストオンリーアダプターでもよいがパッケージマネージャとか動かす必要があると思うのでNATとポートフォワーディングの設定をダブルで入れておくとよいと思う。

4. Docker用の仮想マシン起動

   **Docker Quickstart Terminal**を起動。defaultという仮想マシンが起動する。クジラのアスキーアートみたいなのが出たら起動完了。

5. 実行確認

   試しに下記のようなファイル、ディレクトリを準備。

   whale
   ├ app  // Rails
   ├ db   // MySQLのデータボリュームだけマウントされる場所
   ├ web  // DockerfileとGemfile, Gemfile.lockだけ置いておく
   ├ docker-compose.yml
   ├ start.sh
   ├ bundle.sh
   └ stop.sh

   docker-compose.ymlを置いたディレクトリで下記コマンドを実行(中身は`docker-compose up -d`と`bundler install`してるだけ)。

   ````bash
   $ sh start.sh
   ```

   下記のような表示が出力されればOK。

   ```bash
   Creating whale_db_1_7096d34d8fe6 ... done
   Creating whale_web_1_d4c6fc83efef ... done
   .
   .
   .
   Bundle complete! 26 Gemfile dependencies, 88 gems now installed.
   Bundled gems are installed into `/usr/local/bundle`
   ```
