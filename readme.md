# Ingress Controller

REGION_CODE=us-east-1
CLUSTER_NAME=rajolinaveen
ACC_ID=343430925817

### Permissions

* OIDC provider
```
eksctl utils associate-iam-oidc-provider \
    --region $REGION_CODE \
    --cluster $CLUSTER_NAME \
    --approve
```
* IAM Policy
```
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.12.0/docs/install/iam_policy.json
```
* Create IAM Policy
```
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json
```
* Provide access to EKS through IAM Policy
```
eksctl create iamserviceaccount \
--cluster=$CLUSTER_NAME \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::$ACC_ID:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region $REGION_CODE \
--approve
```

# SERVICE ACCOUNT SETUP
```
eksctl create iamserviceaccount \
--cluster=rajolinaveen \
--namespace=prod \
--name=expense-mysql-secret \
--attach-policy-arn=arn:aws:iam::343430925817:policy/ExpenseMYSQLSecrets \
--override-existing-serviceaccounts \
--region us-east-1 \
--approve

```