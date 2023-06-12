# Vault_EKS_JWT


Enable the JWT/OIDC auth method in HCP Vault using your terminal
vault auth enable jwt

Define the jwt config. When using the jwt auth method, you only need the oidc discovery url and sometimes a TLS certificate authority to trust
vault write auth/jwt/config \
    oidc_discovery_url="https://oidc.eks.us-east-2.amazonaws.com/id/70E6A5C14C513E0BABEE36DD5AF0AA34" \
    default_role="eks"
    
Define the role for a specific service account or many services account. You can create a role per service account if they don't have the same TTL or the policies. 
vault write auth/jwt/role/eks @roleseks.json 

In order to reduce the Vault client, you need to take the 'iss' parameter as the user_claim. The user_claim is tied to the identity of the cluster instead of the subject. The documentation shows the example of attaching the service account to the Vault cluster. The subject (sub) refers to the service account.
You can check the token claims in the jwt.io website to make sure you put the valid claims. 
Keep in mind that using this approach, you will see the cluster's identity as the Vault client. If you do audit on the logs for whatever the reason is, you will not able to see the services account entities, therefore you will not see who service account did what. 

// Create the service account's token to connect to the Vault cluster
kubectl create token saandrea -n andrea
    
