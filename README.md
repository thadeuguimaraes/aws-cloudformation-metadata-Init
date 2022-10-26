# Helper Scripts  cfn-init, cfn-hup and  cfn-signal

# Helper Scripts

#  O AWSCloudFormatio fornece os seguintes scripts helper

que podemos usar para instalar software e iniciar serviços na Amazon EC2
que criamos como parte da pilha.
  - cfn-init
  - cfn-signal
  - cfn-get-metadata
  - cfn-hup
  
==============================================================================

# Metadata AWS::CloudFormation::Init

# Step 00 – Base Template 

Resources 
  - Security Group  
  - VM Instnaces 
- Parameters 
  - Vamos parametrizar o parâmetro KeyName

================================================================================  

# Step-01: Metadata: AWS::CloudFormation::Init 

- Digite AWS :: CloudFormation :: INIT será usado para incluir a seção de metadados em uma instância
  do EC2 para o script auxiliar CFN-Init.
- A configuração é separada nas seções.
- Os metadados são organizados para configurar as teclas, que podemos até agrupar em configurações.
- Por padrão, CFN-Init Chamadas e processa a seção de metadados quando possui uma tecla de configuração
  única (sem configurações de configuração definidas).
- Podemos até especificar o ConfigSets como entrada para o script CFN-Init para que ele possa processar
  todo o ConfigSet com todos os seus ConfigKeys.Veremos isso em detalhes na seção Configsets.
- O script auxiliar CFN Init Processa as seções de configuração na ordem especificada na seção de sintaxe.

# Step-01: Metadata: Structure
- Se quisermos processá -lo em ordem diferente, precisamos
  separe -os em diferentes teclas de configuração e depois use o
  Ordem de execução para teclas de configuração em um conjunto de configurações.
- Nesta etapa, apenas adicionaremos a seção de metadados com
  estrutura.
- Vamos construir incrementalmente as seções de metadados em
  próximas etapas.

==============================================================================================================

# Step-02: Metadata: packages

- We can use packages key to download and install pre-packaged applications. 
- On windows systems packages key supports only the MSI Installer. 
- Supported Package Formats: 
- apt 
- msi 
- python 
- rpm 
- rubygems 
- yum 

================================================================================

# Step-03: Metadata: groups

- Podemos usar grupos para criar grupos Linux/Unix e       atribuir ao grupo
Id's.
- A chave dos grupos não é suportada para sistemas Windows.
- Podemos criar vários grupos conforme necessário.
- Podemos criar sem ID do grupo ou criar com um ID de grupo desejado. 
- Syntax:  

=======================================================================

# Step-04: Metadata: users

- Podemos usar a chave dos usuários para criar usuários Linux/Unix na instância do EC2.
- A chave dos usuários não é suportada para sistemas Windows.
- A seguir estão as keys suportadas
  - uid 
  - groups 
  - homeDir 
- Os usuários são criados como usuários do sistema não interativo com um shell de /sbin /nologin.
- Isso é por design e não pode ser modificado.

=================================================================================================

# Step-05: Metadata: sources

- Podemos usar a key das fontes para baixar um arquivo e desfazer a unpack.
  em um diretório de destino na instância do EC2.
- Essa chave é totalmente suportada para os sistemas Linux e Windows.
- Formatos de arquivo suportados
  - tar
  - tar + gzip
  - tar + bz2
  - zip
  - Syntax / Example:

# Step-05: Metadata: sources

- Create S3 bucket 
- Disable block public access to bucket. 
- Create cfn folder 
- Upload the zip files demo1.zip, demo2.zip which contains demo.war (two versions v1 and v2)
  - Unzip AWS-CloudFormation.zip to local directory 
  - Navigate to 11-cfn-init/WAR-Files folder 
  - Upload the demo1.zip, demo2.zip to S3 bucket cfn folder.  
  - Path: /AWS-CloudFormation/11-cfn-init/WAR-files 
  - Make the demo1.zip, demo2.zip as public file. 
  - Copy the S3 http url for both files and perform public access test.
  - Update demo1.zip url in sources section of template.

=====================================================================================================

# Step-06: Metadata: files

- Podemos usar a key de arquivos para criar arquivos na instância do EC2.
- O conteúdo pode estar em linha no modelo ou o conteúdo pode ser extraído de um URL.
- Os arquivos são gravados no disco em ordem alfabética.
- Key suportadas
  - content
  - source
  - Encoding (plain or base64)
  - group
  - owner
  - mode
  - authentication
  - contex

=============================================================================================

# Step-07: Metadata: commands

- Podemos usar a key de comandos para executar
  comandos na instância do EC2.
- Os comandos são processados em
  Ordem alfabética por nome.
- Supported Keys 
  - command 
  - env 
  - cwd 
  - test 
  - ignoreErrors 
  - waitAfterCompletion

=================================================================================================

# Step-08: Metadata: services

- Podemos usar a key de serviços para definir quais serviços devem ser
  ativados ou desativados quando a instância for iniciada.
- Nos sistemas Linux, essa chave é suportada usando o sysvinit.
- Nos sistemas Windows, ele é suportado usando o Windows Service Manager.
- A Key de serviços também nos permite especificar dependências de fontes, pacotes e
  arquivos para que, se um reinício for necessário devido à instalação de arquivos, o
  CFN-Init pode cuidar do reinício do serviço.
- Keys suportadas
  - ensureRunning
  - enabled
  - files
  - sources
  - packages
  - commands

 # Step-08: Metadata: services

- O serviço nginx será reiniciado se qualquer um
  /etc/nginx/nginx.conf ou/var/www/html são
  modificado por cfn-init.
- O serviço PHP-FASTCGI será reiniciado se cfn-
  o init instala ou atualiza PHP ou Spawn-fcgi usando
yum.
- O serviço de sendmail será interrompido e
  Desativado.

==========================================================================================================
  # UserData

# Step-09: UserData: aws-cfn-bootstrap

- Os scripts auxiliares são atualizados periodicamente.
- Precisamos garantir que o comando listado abaixo esteja incluído em
  Dados do usuário do nosso modelo antes de ligar para os scripts auxiliares para garantir
  que nossas instâncias lançadas obtêm os mais recentes scripts auxiliares.

============================================================================================================

# Step-10: UserData: cfn-init

- O script cfn-init template metadata do AWS::CloudFormation::Init 
  key age de acordo com:
  - buscar e analisar metadados da AWS CloudFormation
  - Instale pacotes
  - Escreva arquivos no disco
  - Ativar/desativar e iniciar/parar os serviços
  - Se usarmos o CFN-Init para atualizar um arquivo existente, ele cria uma cópia de backup do arquivo
    original no mesmo diretório com uma extensão .bak.
  - O CFN-Init não requer credenciais.No entanto, se nenhuma credenciais for especificada, o AWS CloudFormation
    Verifica a associação da Stack e limita o escopo da chamada para a pilha a que a instância pertence.

================================================================================================================

# Step-11: UserData: cfn-signal

- O script auxiliar do Signal CFN sinaliza AWS
   Formação em nuvem para indicar se
   As instâncias do Amazon EC2 foram
   criada ou atualizada com sucesso.
- Se instalarmos e configurarmos o software
   aplicações em instâncias, podemos sinalizar o
   AWS CloudFormation quando esses softwares
   aplicações estão prontas.
- Podemos usar o script de signal CFN em
   Conjunção com uma CreationPolicy.

# Step-11: UserData: cfn-hup

- Nota importante: daqui em diante, começaremos a criar a pilha usando
    arquivo de modelo v12, adicionaremos o comando cfn-hup também ao modelo
    seção de dados do usuário, apesar de discutirmos essa seção na etapa 14.
    a razão para fazer isso é que as alterações relacionadas ao usuário devem ser incluídas
    apenas durante o tempo de criação da instância.
- Final Look of UserData:

===============================================================================================

# Step 12 - Outputs

- Adicione saídas no template.
- Adicionaremos a saída Appurl para acessar facilmente o aplicativo após
criação de stack.

# Step 12: Create Stack using template 11-12-cfn-init-v12-Outputs.yml 

- Observações
   - CloudFormation recebe o sinal assim que o recurso de instância da VM recebe
     criada.
   - Em outras palavras, veremos o status da stack "create_complete" mesmo
     embora as instalações do aplicativo de fundo no fundo estejam acontecendo no EC2
     instância.
- Com essa abordagem, temos problemas como
    - As instalações de aplicativos falham e vemos o status da pilha como "create_complete" em verde.
    - Não saberemos o que aconteceu com as instalações ou configurações do nosso aplicativo até que
     faça login na instância.
- Para superar esse tipo de problema, precisamos usar a "política de criação" que nós
verá na próxima etapa (etapa 13).

===================================================================================================

# Step-13: Creation Policy 

- Associe o atributo CreationPolicy a um recurso para impedir que seu status atinja o Create completo até que a AWS CloudFormation receba um número especificado de sinais de sucesso ou o período de tempo limite seja excedido.
- Para sinalizar um recurso, podemos usar o script auxiliar CFN-Signal.
- A política de criação é invocada apenas quando a AWS CloudFormation cria o recurso associado.
- Atualmente, os únicos recursos da AWS CloudFormation que suportam políticas de criação são
  - AWS::AutoScaling::AutoScalingGroup  
  - AWS::EC2::Instance
  - AWS::CloudFormation::WaitCondition

# Step-13: Creation Policy 

- Use o atributo CreationPolicy quando quiser esperar no recurso
as ações de configuração antes da criação da stack prosseguir.
- por exemplo, se instalarmos e configurarmos aplicativos de software em um
Instância do EC2, podemos querer que esses aplicativos estejam em execução antes
processo.Nesses casos, podemos adicionar um atributo CreationPolicy a
a instância e depois enviar um sinal de sucesso para a instância após o
os aplicativos são instalados e configurados.

======================================================================================================================================

# Step-14: UserData: cfn-hup

- CFN-Hup Helper é um daemon que detecta mudanças no recurso
metadados e executa ações especificadas pelo usuário quando uma alteração o
detectou.
- Isso nos permite fazer atualizações de configuração em nossa execução
Instância do EC2 através do recurso de pilha de atualização.
- cfn-hup.conf
- CFN-HUP.CONF Arquivo armazena o nome da pilha e da AWS
Credenciais que os daemon CFN-HUP metas.
- formato de cfn-hup.conf
- Estamos criando este arquivo usando nossa key de metadados nomeada arquivos em
noso template.

# Step-14: UserData: cfn-hup

- cfn-hup.conf file content 
  - stack 
  - credential-file 
  - role 
  - region 
  - umask (default: 022) 
  - Interval (default: 15) 
  - Verbose 
- hooks.d directory 
  - Para suportar a composição de vários aplicativos que implantam hooks de notificação de alteração, o CFN-HUP suporta um diretório chamado Hooks.d que está localizado no diretório de configuração do Hooks.
   - Podemos colocar um ou mais arquivos de configuração de hooks adicionais no diretório Hooks.d.

# Step-14: UserData: cfn-hup - hooks.conf

Step-14: UserData: cfn-hup - hooks.conf

-As ações do usuário que o daemon cfn-hup chama periodicamente são definidas em
Hooks.conf.
- Syntax:
"/etc/cfn/hooks.d/cfn-auto-reloader.conf":
-              content: !Sub |
-                [cfn-auto-reloader-hook]
-                triggers=post.update
-                path=Resources.MyVMInstance.Metadata.AWS::CloudFormation::Init
-                action=/opt/aws/bin/cfn-init -v --stack ${AWS::StackName} --resource MyVMInstance --region ${AWS::Region}
-              mode: "000400"
-              owner: "root"
-              group: "root"

# Step-14: UserData: cfn-hup - hooks.conf 

- Quando a ação é executada, ela é executada em uma cópia do ambiente atual
(que CFN-hup está dentro), com CFN_OLD_METADATATA definido para o anterior
valor do caminho e CFN_New_Metadata define como o valor atual.
- O arquivo de configuração de hooks é carregado na startup de daemon cfn-hup
somente, os hooks novos exigirão que o daemon seja reiniciado.
- Um cache dos valores de metadados anteriores é armazenado em/var/lib/cfn-
hup/dados/metadata_db
- Podemos excluir este cache para forçar o CFN-hup a executar todas as ações postais
novamente.

# Step 14: Create Stack using template 11-14-cfn-init-v14-Update-App.yml 

- Observações
  - O antigo WAR de guerra será removido
  - O novo arquivo WAR será implantado com sucesso.
  - Quando acessarmos a nova versão do aplicativo do conteúdo o aplicativo será
exibido.

# Metadata cfn-init -Conclusão

Então, primeiro foi criado  um template base e, em seguida, criamos o formato de metadados no modelo
E então adicionamos os pacotes que vamos instalar, nada além de Tomcat, JDK e depois JIRA.
Então, criamos alguns grupos, criamos alguns usuários e criamos a fonte.
Na fonte, garantimos que criamos o bucket do S3 e, em seguida, carregamos nossa demonstração one dot zip
e, em seguida, demo de dois pontos zip.
os arquivos dentro desse e então também atualizei o arquivo var o arquivo zip a ser mapeado.
A URL do histórico completo é mapeada nas packes sources for /temp para que assim que os metadados executados sejam processados ​​pelo CFN e init, o demo one dot zip precisa ser inserido no local /temp e
então, adicionamos a seção de arquivos.
Portanto, na seção de arquivos, adicionamos um ícone e, em seguida, vemos se um recarregamento automático ou ícone é exigido pelo jump CFN.
E também analisamos quais são os arquivos, são as chaves e todas essas coisas.
Então passamos para os comandos e
criamos quatro comandos em relação ao que precisamos fazer na instalação de nossa aplicação.Como um é dar as permissões para nosso demo Artois e o segundo é dar as arrays desse JDK sete binários ou arquivo WAR e depois também copiar a barra arquivo de temp para a pasta de aplicativos da web e também remova a estatística de demonstração de demonstração antiga, o que significa a barra de demonstração e a pasta de demonstração dos aplicativos da web.
Quando estamos fazendo as atualizações, precisamos remover isso, certo?
Portanto, temos além de serviços, o que vimos é que precisamos iniciar o Tomcat Service. Já vimos comandos em detalhes lá. então, adicionamos o Tomcat Service na seção de serviços.
Além disso, as estimativas de mudança, como os sources podem ser reiniciados sempre que a fonte significa, como podemos adicionar a seção de fontes ou a seção de pacotes, ou podemos adicionar a seção de arquivo em sources.
Sempre que ocorrerem alterações na respectiva pasta do respectivo arquivo, o serviço precisará ser reiniciado.
então, discutimos em detalhes sobre essas coisas e, em seguida, passamos da seção de metadados para a seção de bootstrap.
Na seção Bootstrap O que fizemos foi adicionar um comando que atualiza os scripts auxiliares durante a instância, durante o início de nossa instância de VM
sempre que a instância de VM estiver sendo criada, ela receberá os scripts de bootstrap CFN mais recentes.
E então atualizamos esse aspecto para ver se um comando init em nosso template. 
Da mesma forma, também adicionamos as saídas em nosso modelo e, em seguida, criamos uma stack e vimos nossas observações sobre como sinalizar CFN e, em seguida, a política de criação pode funcionar melhor.
e aqui, a esse respeito, a seção de criação de pilha de saídas, não vimos esse desempenho melhor, o que significa que não funcionou melhor porque a instalação do aplicativo não aconteceu. Mas a formação da nuvem disse que a criação da pilha está completa. esse tipo de desvantagem que vimos quando criamos a pilha aqui.
Então, o que fizemos na próxima seção é que adicionamos a política de criação e então criamos uma nova pilha aqui. Já vimos isso.
A formação da nuvem, disse Stack concluída apenas quando a instalação do aplicativo de software aplicativo foi concluída dentro do SC duas instância.
Então passamos para ver se um aconteceu e discutimos em detalhes o que é KAF e qual é o seu intervalo, como ele vai funcionar, todas essas coisas em detalhes. vimos que o aplicativo V dois foi exibido. Uma coisa importante que notamos aqui é que CF e hub precisam ser feitos no intervalo de tempo. Então, o que significa que precisamos esperar até que o intervalo máximo seja o período de intervalo máximo para que nosso mudanças foram refletidas ou não serão conhecidas.
Então, isso é sobre o alto nível que fizemos para a seção completa do cf init.