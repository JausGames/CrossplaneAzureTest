`sudo snap install kubectl --classic`
`sudo snap install helm --classic`
`minikube start`
`kubectl create namespace crossplane-system`
`helm repo add crossplane-stable https://charts.crossplane.io/stable`
`helm repo update`
`helm install crossplane --namespace crossplane-system crossplane-stable/crossplane`
`curl -sL https://raw.githubusercontent.com/crossplane/crossplane/release-1.0/install.sh | sh`
`sudo mv kubectl-crossplane /usr/local/bin`
`kubectl crossplane install provider crossplane/provider-azure:v0.14.0`
`curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`
`az login`
`az ad sp create-for-rbac --role Owner --scopes az ad sp create-for-rbac --role Owner --scopes /subscriptions/20fb29c2-7546-4100-ab06-6e47cb7f0ac5 --sdk-auth > "creds.json" --sdk-auth > "creds.json"`
`if which jq > /dev/null 2>&1; then
>   AZURE_CLIENT_ID=$(jq -r ".clientId" < "./creds.json")
> else
>   AZURE_CLIENT_ID=$(cat creds.json | grep clientId | cut -c 16-51)
> fi
RW_ALL_APPS=cf
RW_DIR_DATA=78c8a3c8-a07e-4b9e-af1b-b5ccab50a175
AAD_GRAPH_API=00000002-0000-0000-c000-000000000000`
`az ad app permission add --id "${AZURE_CLIENT_ID}" --api ${AAD_GRAPH_API} --api-permissions ${RW_ALL_APPS}=Role`

After executing this command, you will still need to grant the permission through the Azure portal, either via the "Grant permissions" button on the app registration page or by going to the "API permissions" page and clicking "Grant admin consent for {tenant name}".

`az ad app permission grant --id "${AZURE_CLIENT_ID}" --api ${AAD_GRAPH_API} > /dev/null`

`kubectl create secret generic azure-creds -n crossplane-system --from-file=key=./creds.json`
`kubectl apply -f ProviderConfig.yaml`
`kubectl apply -f aks.yaml`


"oauth2PermissionScopes": [
    {
      "adminConsentDescription": "Allows the app to read the memberships of hidden groups and administrative units on behalf of the signed-in user, for those hidden 
groups and administrative units that the signed-in user has access to.",
      "adminConsentDisplayName": "Read hidden memberships",
      "id": "2d05a661-f651-4d57-a595-489c91eda336",
      "isEnabled": true,
      "type": "Admin",
      "userConsentDescription": "Allows the app to read the memberships of hidden groups or administrative units on your behalf, for those hidden groups or administrative units that you have access to.",
      "userConsentDisplayName": "Read your hidden memberships",
      "value": "Member.Read.Hidden"
    },
    {
      "adminConsentDescription": "Allows users to sign in to the app, and allows the app to read the profile of signed-in users. It also allow the app to read basic 
company information of signed-in users.",
      "adminConsentDisplayName": "Sign in and read user profile",
      "id": "311a71cc-e848-46a1-bdf8-97ff7156d8e6",
      "isEnabled": true,
      "type": "User",
      "userConsentDescription": "Allows you to sign in to the app with your work account and let the app read your profile. It also allows the app to read basic company information.",
      "userConsentDisplayName": "Sign you in and read your profile",
      "value": "User.Read"
    },
    {
      "adminConsentDescription": "Allows the app to read a basic set of profile properties of all users in your company or school on behalf of the signed-in user. Includes display name, first and last name, photo, and email address. Additionally, this allows the app to read basic info about the signed-in user's reports and manager.",
      "adminConsentDisplayName": "Read all users' basic profiles",
      "id": "cba73afc-7f69-4d86-8450-4978e04ecd1a",
      "isEnabled": true,
      "type": "User",
      "userConsentDescription": "Allows the app to read a basic set of profile properties of other users in your company or school, on your behalf. Includes display 
name, first and last name, photo, and email address. Additionally, this allows the app to read basic info about your reports and manager.",
      "userConsentDisplayName": "Read all user's basic profiles",
      "value": "User.ReadBasic.All"
    },
    {
      "adminConsentDescription": "Allows the app to read the full set of profile properties of all users in your company or school, on behalf of the signed-in user. 
Additionally, this allows the app to read the profiles of the signed-in user's reports and manager.",
      "adminConsentDisplayName": "Read all users' full profiles",
      "id": "c582532d-9d9e-43bd-a97c-2667a28ce295",
      "isEnabled": true,
      "type": "Admin",
      "userConsentDescription": "Allows the app to read the full set of profile properties of all users in your company or school, on your behalf.  Additionally, this allows the app to read the profiles of your reports and manager.",
      "userConsentDisplayName": "Read all user's full profiles",
      "value": "User.Read.All"
    },
    {
      "adminConsentDescription": "Allows the app to read basic group properties and memberships on behalf of the signed-in user.",
      "adminConsentDisplayName": "Read all groups",
      "id": "6234d376-f627-4f0f-90e0-dff25c5211a3",
      "isEnabled": true,
      "type": "Admin",
      "userConsentDescription": "Allows the app to read all group properties and memberships on your behalf.",
      "userConsentDisplayName": "Read all groups",
      "value": "Group.Read.All"
    },
    {
      "adminConsentDescription": "Allows the app to create groups on behalf of the signed-in user and read all group properties and memberships. Additionally, this allows the app to update group properties and memberships for the groups the signed-in user owns.",
      "adminConsentDisplayName": "Read and write all groups",
      "id": "970d6fa6-214a-4a9b-8513-08fad511e2fd",
      "isEnabled": true,
      "type": "Admin",
      "userConsentDescription": "Allows the app to create groups on your behalf and read all group properties and memberships. Additionally, this allows the app to update group properties and memberships for groups you own.",
      "userConsentDisplayName": "Read and write all groups",
      "value": "Group.ReadWrite.All"
    },
    {
      "adminConsentDescription": "Allows the app to read and write data in your company or school directory, such as users, and groups.  Does not allow user or group deletion.",
      "adminConsentDisplayName": "Read and write directory data",
      "id": "78c8a3c8-a07e-4b9e-af1b-b5ccab50a175",
      "isEnabled": true,
      "type": "Admin",
      "userConsentDescription": "Allows the app to read and write data in your company or school directory, such as other users, groups.  Does not allow user or group deletion on your behalf.",
      "userConsentDisplayName": "Read and write directory data",
      "value": "Directory.ReadWrite.All"
    },
    {
      "adminConsentDescription": "Allows the app to read data in your company or school directory, such as users, groups, and apps.",
      "adminConsentDisplayName": "Read directory data",
      "id": "5778995a-e1bf-45b8-affa-663a9f3f4d04",
      "isEnabled": true,
      "type": "Admin",
      "userConsentDescription": "Allows the app to read data in your company or school directory, such as other users, groups and apps.",
      "userConsentDisplayName": "Read directory data",
      "value": "Directory.Read.All"
    },
    {
      "adminConsentDescription": "Allows the app to have the same access to information in the directory as the signed-in user.",
      "adminConsentDisplayName": "Access the directory as the signed-in user",
      "id": "a42657d6-7f20-40e3-b6f0-cee03008a62a",
      "isEnabled": true,
      "type": "User",
      "userConsentDescription": "Allows the app to have the same access to information in your work or school directory as you do.",
      "userConsentDisplayName": "Access the directory as you",
      "value": "Directory.AccessAsUser.All"
    },
    {
      "adminConsentDescription": "Allows the app to read your organization's policies on behalf of the signed-in user.",
      "adminConsentDisplayName": "Read your organization's policies",
      "id": "80e5b1bf-3ad0-4365-943a-0ec983009b67",
      "isEnabled": true,
      "type": "Admin",
      "userConsentDescription": "Allows the app to read your organization's policies on your behalf.",
      "userConsentDisplayName": "Read your organization's policies",
      "value": "Policy.Read.All"
    }
  ],