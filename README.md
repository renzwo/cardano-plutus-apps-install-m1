# cardano-plutus-apps-install-m1
I was able to get nix and plutus apps running on mac m1 thanks to

https://cardano.stackexchange.com/questions/6287/lessons-learned-setting-up-plutus-playground-feedback-welcome
and
https://github.com/Til-D/cardano-plutus/blob/main/README.md

## Step by step

1
```console
sh <(curl -L https://nixos.org/nix/install) --darwin-use-unencrypted-nix-store-volume
```
2 restart terminal
3
```console
sudo nano /etc/nix/nix.conf
```
4 be sure these lines are all inside the file
```console
build-users-group = nixbld

substituters        = https://hydra.iohk.io https://iohk.cachix.org https://cache.nixos.org/
trusted-public-keys = hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=

sandbox = true
extra-sandbox-paths = /System/Library/Frameworks /System/Library/PrivateFrameworks /usr/lib /private/tmp /private/var/tmp /usr/bin/env
experimental-features = nix-command
extra-experimental-features = flakes
```
if this does produce an error in step 9, plz also insert these lines
```console
system = x86_64-darwin
extra-platforms = x86_64-darwin aarch64-darwin
```
5 restart mac
6 clone the right repository! not the ...plutus one, it needs to be the ...plutus-apps repo:
```console
git clone https://github.com/input-output-hk/plutus-apps
```
7 goto folder
```console
cd plutus-apps/
```
8 checkout latest commit (plz update for yourself)
```console
git checkout 7f53f18dfc788bf6aa929f47d840efa1247e11fd
```
9 build the server
```console
nix-build -A plutus-playground.server
```
10 build the client
```console
nix-build -A plutus-playground.client
```
11 start nix shell (takes some time)
```console
nix-shell
```
12 goto server dir and start server
```console
cd plutus-playground-server
plutus-playground-server
```
13 wait until server is started! you will see something like this
```console
[nix-shell:~/Documents/Source/plutus-apps/plutus-playground-server]$ plutus-playground-server
plutus-playground-server: for development use only
[Info] Running: (Nothing,Webserver {_port = 8080, _maxInterpretationTime = 80s})
Initializing Context
Initializing Context
Warning: GITHUB_CLIENT_ID not set
Warning: GITHUB_CLIENT_SECRET not set
Warning: JWT_SIGNATURE not set
Interpreter ready
```
14 when server is started you can open a second nix-shell in another terminal
```console
nix-shell
```
15 in the second terminal with nix-shell run
```console
sudo npm install -g npm
```
16 goto client dir and start client
```console
cd plutus-playground-client
npm run start
```
17 wait until client has started! will take some time.

18 finished. goto https://localhost:8009/ 
