Installation de Kubectl : `sudo snap install kubectl --classic`

Installation de helm : `sudo snap install helm --classic`

Lancement du conteneur minikube : `minikube start`

Tout d'abord, nous devons installer Corssplane sur un cluster Kubernetes. Je vais utiliser Crossplane sur mon cluster Docker, puis mettre en place l'infrastructure sur Azure :
`kubectl create namespace crossplane-system`
`helm repo add crossplane-stable https://charts.crossplane.io/stable`
`helm repo update`
`helm install crossplane --namespace crossplane-system crossplane-stable/crossplane`

Ensuite, nous pouvons installer Crossplane client qui sera utilisée en combinaison avec kubectl :
`curl -sL https://raw.githubusercontent.com/crossplane/crossplane/master/install.sh | sh`
`sudo mv kubectl-crossplane /usr/local/bin`

On installe le provider azure de crossplane :
`kubectl crossplane install provider crossplane/provider-azure`

On installe azure client :
`curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`

On se connecte à azure : 
`az login`

On crée un fichier creds.json et un client owner :
`az ad sp create-for-rbac --name Owner --sdk-auth > "creds.json"`

On install jk :
`sudo apt install jk`

On assigne les variables:
`AZURE_CLIENT_ID=$(jq -r ".clientId" < "./creds.json")`
`RW_ALL_APPS=2d05a661-f651-4d57-a595-489c91eda336`
`AAD_GRAPH_API=00000002-0000-0000-c000-000000000000`

On ajoute la permission :
`az ad app permission add --id "${AZURE_CLIENT_ID}" --api ${AAD_GRAPH_API} --api-permissions ${RW_ALL_APPS}=Role`

After executing this command, you will still need to grant the permission through the Azure portal, either via the "Grant permissions" button on the app registration page or by going to the "API permissions" page and clicking "Grant admin consent for {tenant name}".

`az ad app permission grant --id "${AZURE_CLIENT_ID}" --api ${AAD_GRAPH_API} --scope user.read.all> /dev/null`
ERROR: Insufficient privileges to complete the operation.
`az ad app permission admin-consent --id "${AZURE_CLIENT_ID}`

`kubectl create secret generic azure-creds -n crossplane-system --from-file=key=./creds.json`
`kubectl apply -f ProviderConfig.yaml`
`kubectl apply -f aks.yaml`