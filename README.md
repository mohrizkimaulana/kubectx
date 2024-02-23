# kubectx
kubectx installation steps for Mac

[INFO]   if `kubectl` is not installed, please refer `KUBECTL INSTALLATION`, if you have installed you can move to `KUBECTX INSTALLATION`

------------------------------------------------------------------

`KUBECTL INSTALLATION`


1. Install kubectl for Mac
   ```https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/```
2. Create config file for kubeconfig
   
   ```bash
   vi $HOME/.kube/config
   ```

4. Add kubeconfig that you get from kube cloud configuration (AWS, GCP, Alicloud and etc), and paste it there. Below is example of config file.

   ```bash
    apiVersion: v1
    clusters:
    - cluster:
        certificate-authority-data: <ca-data-here>
        server: https://your-k8s-cluster.com
      name: <cluster-name>
    contexts:
    - context:
        cluster:  <cluster-name>
        user:  <cluster-name-user>
      name:  <your-context-name>
    current-context:  <cluster-name>
    kind: Config
    preferences: {}
    users:
    - name:  <cluster-name-user>
      user:
        token: <secret-token-here>
   
   ```
5. Check the configuration using kubectl command. Here we try to get the namespace within the cluster
   ```bash
   kubectl get namespace
   ```

------------------------------------------------------------------

`KUBECTX INSTALLTION`

1. Install kubect binary for Mac
   
   ```bash
   brew install kubectx
   ```
2. Check the installation with command below, and you will see the active context(like minikube (if you have installed minikube) or your context name)
   ```bash
   kubectx
   ```
3. Oke Great!. let's try to add new context from another cluster (maybe you have another cluster config)

4. Add new config file, below example we try to create new file on `~`
   
   ```bash
   nano ~/config-name
   ```

   add config-name to /.kube/config
   
   ```bash
   export KUBECONFIG=~/.kube/config:~/config-name
   ```

   ```bash
   kubectl config view --flatten > ~/.kube/.config
   ```

   ```bash
   kubectl krew install konfig
   ```
   add the code below to your .zshrc or .bashrc

   if you use `.zshrc`
   
   ```bash
   nano ~/.zshrc
   ```

   if your use  `.bashrc`

   ```bash
   nano ~/.bashrc
   ```
   
   paste code bellow
   
   ```bash
   export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
   ```

   ```bash
   source ~/.zshrc
   ```

   or

   ```bash
   source ~/.bashrc
   ```

   ###############
   
   if you got an error `error: unknown command "krew" for "kubectl" ` when execute `kubectl krew install konfig`, you need to follow this command below

   ```bash
   nano install.sh
   ```
   
   ```bash
   (
   set -x; cd "$(mktemp -d)" &&
   OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
   ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
   KREW="krew-${OS}_${ARCH}" &&
   curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
   tar zxvf "${KREW}.tar.gz" &&
   ./"${KREW}" install krew
   )
   ```

   ```bash
   chmod +x install.sh
   ```

   ```bash
   ./install.sh
   ```
   
6. Import config file

   ```bash
   kubectl konfig import -s ~/config-name
   ```





