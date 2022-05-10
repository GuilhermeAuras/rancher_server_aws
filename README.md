# rancher_server_aws

* Crie 3 instancias EC2 t3.medium	na Aws e acesse as mesmas via terminal com ssh:
<br>rancher
<br>k8s-1
<br>k8s-2
<br>Importante que no grupo de segurança deve-se liberar a porta 22/tcp para internet

* Instale o git nas 3 instancias criadas:
<br>apt-get update && apt-get install git -y

* Clone o repositorio nas 3 instancias criadas:
<br>git clone https://github.com/GuilhermeAuras/shell_scripts_k8s.git

* Na instancia rancher server pelo terminal execute o script:
<br>bash shell_scripts_k8s/rancher_server_e_k8s/rancher_server.sh

* Nas instancias k8s-1 e k8s-2 pelo terminal execute:
<br>bash shell_scripts_k8s/rancher_server_e_k8s/k8s.sh

* Na instancia rancher server pelo terminal rode o comando abaixo e copie o "Bootstrap Password":
<br>docker logs rancher | grep "Bootstrap Password:"

* Acesse pelo navegador com IP externo da instancia rancher server, e copie o "Bootstrap Password" no primeiro acesso.

* Marque a opção "Set specifc password to use" e coloque sua senha.

* Acesse:
<br>https://ip_da_sua_instancia_ec2_rancher_server/dashboard/c/local/manager/provisioning.cattle.io.cluster/create

* Marque a opção "custom"
<br>Preencha:
<br>Cluster Name

* Depois "Next"

* No passo 1 marque:
<br>etcd
<br>Control Plane
<br>Work

* Done

* No passo 2 copie o "Token".

* Pelo terminal cole o "Token" nas instancias k8s-1 e k8s-2 e aguarde o container subir.

* No rancher server pela interface web cline em "Done" e aguarde terminar.

* No rancher server pela interface web copie o "kubeConfig do cluster".

* No rancher server pelo terminal faça:
<br>vim /root/.kube/config e cole o "kubeConfig do cluster" e salve.

* Teste a comunicação entre os nodes via terminal no rancher server:
<br>kubectl get nodes

* No Rancher server pelo terminal baixe o projeto:
<br> git clone https://github.com/GuilhermeAuras/yamls_scripts_k8s.git

* Check os arquivos:
<br>ls  /yamls_scripts_k8s/site_projeto_nginx/
<br>deployment-frontend.yaml  ingress-frontend.yaml  services-frontend.yaml

* Suba o deploy no rancher server:
<br>kubectl apply -f /yamls_scripts_k8s/site_projeto_nginx/

* Configure o DNS Route 53 (ingress):
<br>rancher.seudominio.com	A	Simples	1.2.3.4 #IP do rancher server
<br>*.rancher.seudominio.com	A	Simples 1.1.1.1  #IP do no_1 2.2.2.2 #IP do no_2

* Teste o acesso via IP externo das instancias k8s-1 ou k8s-2 no navegador, deve abrir uma pagina com Nginx se tudo ocorrer bem.
