# Laboratório prático 1: Configurar as conexões para o conector do Microsoft Graph

## Locatários do WWL – Termos de uso

Se você estiver recebendo um locatário como parte de uma entrega de treinamento com instrutor, observe que o locatário é disponibilizado para dar suporte aos laboratórios práticos no treinamento com instrutor.

Os locatários não devem ser compartilhados ou usados para fins fora dos laboratórios práticos. O locatário usado neste curso é um locatário de avaliação e não pode ser usado ou acessado após o fim da aula e não está qualificado para extensão.

Os locatários não podem ser convertidos em uma assinatura paga. Os locatários obtidos como parte deste curso permanecem a propriedade da Microsoft Corporation e reservamos o direito de obter acesso e a qualquer momento.

## Resumo

Neste laboratório, você usará o centro de administração do Microsoft 365 para cria uma conexão com os arquivos do cliente usando o Conector de Compartilhamento de Arquivo da Microsoft.

Imagine que você é gerente de atendimento ao cliente da Contoso, uma empresa de médio porte. Sua equipe está tendo problemas com tempos de resposta e eficiência geral ao lidar com as consultas dos clientes. Você decide usar o centro de administração do Microsoft 365 para criar uma conexão FileShare que permite que seus agentes de atendimento ao cliente acessem e referenciem rapidamente os arquivos do cliente.

## Exercício 1: Configurar as conexões no centro de administração do Microsoft 365

### Fazer logon na VM

Seu provedor de hospedagem de laboratório fornece uma senha para a conta de administrador do MOD, que é o administrador de locatários padrão. Por motivos de segurança, a Microsoft configurou o locatário de avaliação de modo que todos os usuários predefinidos tenham de alterar a senha na próxima entrada. Faça login na **LON-CL1** com a conta de **Administrador** local que foi criada pelo provedor de hospedagem do laboratório com a senha **Pa55w.rd**.

### Tarefa 1: Conceda permissões no portal do Azure

1. Navegue até o portal do Azure **https://www.portal.azure.com** e entre com suas credenciais administrativas. Não salve a senha e selecione **Sim** para **Permanecer conectado?**.
2. Na tela **Boas-vindas ao Microsoft Azure**, clique em **Cancelar**.
1. Clique no ícone do menu suspenso no lado esquerdo da tela para exibir o menu Portal. Selecione **Microsoft Entra ID -> Gerenciar -> Registros de aplicativo**.
1. Na barra de menus, escolha **Novo registro**. A página **Registrar um aplicativo** é exibida. Nesta página, dê um nome para o aplicativo; vamos nomear esse aplicativo de **Compartilhamento de arquivos da Contoso**. Deixe a opção padrão de tipos de conta com suporte como **Somente contas neste diretório organizacional (somente Contoso – Locatário único).** Não escolha um **URI de redirecionamento** opcional.
1. Selecione **Registrar**. Seu aplicativo será criado e uma ID de aplicativo será atribuída a ele. Você usará essas informações ao criar seu GCA (Agente de conector do Graph) nas etapas a seguir. Mas antes de criarmos o GCA vamos definir as configurações necessárias.

A próxima etapa é conceder permissões para o agente do conector do Graph no Portal do Azure:

1. No menu à esquerda, escolha a opção **Gerenciar -> Permissões de API**.
1. Escolha **Adicionar uma permissão -> Microsoft Graph -> Permissões de aplicativo** e permita permissões para as seguintes APIs:

    - Diretório -> Diretório.Read.All
    - ExternalConnection -> ExternalConnection.ReadWrite.OwnedBy
    - ExternalItem -> ExternalItem.ReadWrite.All
      
1. Selecione o botão **Adicionar permissões**.
1. Escolha **Conceder consentimento do administrador para a Contoso** e confirme clicando em **Sim**.

**Observação:** não feche esta guia do navegador Edge. Você terá que copiar e colar informações do portal do Azure para as tarefas a seguir.

### Tarefa 2: Instale o GCA

1. Abra uma nova guia do navegador Microsoft Edge. Navegue até a seguinte URL para baixar o Agente do conector do Graph: **https://www.microsoft.com/en-us/download/details.aspx?id=104045**. Selecione o botão **Baixar** . 
1. Abra o arquivo **GcaInstaller_3.1.1.0.msi** e siga os prompts no assistente de instalação. 
2. Na barra **Pesquisar** na parte inferior da tela, insira **Configuração do agente do conector do Graph** e selecione o aplicativo no menu quando ele aparecer.
3. Para permitir que o aplicativo faça alterações no dispositivo, clique em **Sim**.
4. Faça login e registre o GCA usando a conta **administrativa do MOD**. Uma confirmação de que a autenticação foi concluída é exibida no navegador Edge. Você pode fechar esta janela.
5. Abra o aplicativo GCA clicando no ícone na parte inferior da tela.
1. Nomeie esse agente como **ContosoFiles**.
1. Selecione a guia Microsoft Edge do portal do Azure (você verá como **Compartilhamento de arquivos da Contoso – Microsoft Azure**), navegue até a tela **Visão geral** e copie a **ID do aplicativo (cliente)**. Cole-a no aplicativo de instalação do GCA.

Em seguida, você precisa definir um Segredo do Cliente para este aplicativo no portal do Azure.

1. Retorne à guia do Edge no portal do Azure e navegue até **Certificados e segredos**.
1. Selecione **+ Novo segredo do cliente**. Adicione uma **Descrição** inserindo **Arquivos da Contoso**. Você pode deixar o campo de expiração definido como o padrão de 180 dias.
2. Selecione **Adicionar**.
3. Copie o campo **Valor** do segredo silencioso.
1. Retorne ao aplicativo do agente do conector do Graph e cole o valor no campo **Senha do aplicativo (segredo do cliente)** do aplicativo instalador do GCA.
1. Selecione **Registrar**.
1. Assim que o registro for concluído, feche o aplicativo instalador.

### Tarefa 3: Baixe os arquivos de recurso no Github

Para configurar o conector usando o GCA, você precisará de arquivos locais do sistema. 

1. Abra um novo navegador Edge e, na barra de endereços, digite **https://github.com/MicrosoftLearning/MS-4014-Build-a-foundation-to-extend-Microsoft-365-Copilot/tree/master/ResourceFiles**.
2. Escolha o primeiro arquivo na pasta, **Contoso Chai Tea market trends 2023.xls** para abri-lo.
3. Clique no botão de **reticências (mais ações de arquivo)** no canto superior direito da tela e clique em **Baixar**.
4. Repita a etapa 3 para cada um dos arquivos restantes.
5. Você pode fechar esta janela assim que os downloads forem concluídos.
6. Use o Explorador de Arquivos e navague até o diretório C:\. Crie uma nova pasta chamada **ResourceFiles**.
7. Abra uma nova janela do explorador de arquivos e navegue até a pasta **Download** para acessar os arquivos da Contoso que você baixou do GitHub. Copie esses arquivos para o diretório **C:\ResourceFiles**.

Você usará esses arquivos como o conteúdo de origem para a conexão criada no centro de administração da Microsoft.

### Tarefa 4: Abra o Centro de administração da Microsoft

1. Abra uma nova guia do navegador Microsoft Edge, vá para a **Página inicial do Microsoft 365** inserindo o seguinte URL na barra de endereços: **https://portal.office.com**
1. Se for solicitado que você entre, insira o **Nome de usuário** LON-CL1 e a **Senha** fornecidos pelo provedor de hospedagem de laboratório para seu locatário de avaliação do Microsoft 365. O nome de usuário deve estar no formato de **<admin@xxxxxZZZZZZ.onmicrosoft.com>**, onde xxxxxZZZZZZ é o prefixo do locatário atribuído pelo seu provedor de hospedagem de laboratório. 
1. A página **Boas-vindas ao Microsoft 365** aparecerá no navegador Microsoft Edge na guia **Página Inicial | Microsoft 365**. Esta é a home page do Microsoft 365 do MOD Administrator.
1. Na lista de ícones de aplicativos que aparece no painel de navegação, escolha **Administrador**; essa ação abrirá o **centro de administração do Microsoft 365** em uma nova guia do navegador.
1. Selecione **... Mostrar tudo** para exibir o menu de navegação completo. Selecione **Configurações** -> **Pesquisa e inteligência.**
1. Na guia **Visão geral** exibida, role para baixo. Selecione **Adicionar sua primeira conexão** na caixa **Conectores**.

Uma lista de fontes de dados disponíveis é listada. Estamos configurando uma conexão usando o conector de compartilhamento de arquivos. Esse conector permite que a pesquisa do Microsoft 365 Copilot faça referência a arquivos do cliente em uma pesquisa do Copilot e permite tempos de resposta mais rápidos e respostas mais precisas à medida que um agente de atendimento ao cliente encontra respostas.

1. Selecione **Compartilhamentos de arquivo** e, em seguida, **Avançar**.
1. Insira o **Nome de exibição**. O nome de exibição é um nome exclusivo que é exibido aos usuários. Vamos inserir **Arquivos da Contoso** no campo.
1. Insira o **Caminho da pasta**. Temos arquivos de amostra configurados no seguinte diretório:

   **C:\ResourceFiles**

O caminho é o diretório onde os arquivos estão armazenados.

1. Selecione o GCA que criamos nas etapas anteriores (ContosoFiles) no campo **Agente do conector do Graph** se não for a opção padrão.
1. Escolha **Windows** como o Tipo de autenticação e insira o **Nome de usuário** LON-CLI e a **Senha** para autenticação do Windows.
1. Marque a caixa de seleção para autorizar a Microsoft a criar um índice de dados de terceiros em seu locatário.
1. Selecione **Criar**.  Uma tela de sucesso será exibida e a conexão começará a ser sincronizada. Você pode inserir o tipo específico de conteúdo, os departamentos da sua organização que podem se beneficiar de seu uso ou exemplos de fluxos de trabalho na caixa **Descrição do conector**. Por exemplo:

    **Esse conector contém informações contidas no servidor de compartilhamento de arquivos local. Inclui perfis de clientes e perguntas de clientes que podem beneficiar o suporte técnico ao responder às consultas dos clientes.**
1. Selecione **Concluído**. Sua conexão agora aparecerá na guia **Pesquisa e inteligência** do centro de administração do Microsoft 365 e pode ser referenciada nos resultados da pesquisa ou quando você criar um agente do Microsoft 365 Copilot.
