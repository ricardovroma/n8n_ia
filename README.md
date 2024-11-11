# n8n_ia

## Descrição

Este projeto cria uma stack (muito) básica de n8n selfhosted com o intuito de criar um workflow simples de interação com LLM's. 
    
    - **Observação:** Esta stack foi projatada tendo em mente a interação com Ollama rodando localmente

## Setup Ollama

### Instalação & pull de models utilizados

    curl -fsSL https://ollama.com/install.sh | sh
    ollama run llama3.1

### Expondo Ollama para interalçao com n8n dockerizado

    - Editar host Ollama:

        sudo systemctl edit ollama.service 

    - inserir seguintes linhas:

        [Service]

        Environment="OLLAMA_HOST=0.0.0.0"
    
    - Reiniciar daemon e Ollama:

        sudo systemctl daemon-reload && sudo systemctl restart ollama
    
    - **Opcional** Testar conexão:

        curl http://<ip_da_sua_maquina>:11434/api/generate -d '
        {  
        "model": "tinyllama",  
        "prompt": "Why is the blue sky blue?",  
        "stream": false,
        "options":{
        "num_thread": 8,
        "num_ctx": 2024
        }
        }'  | jq .

    - Configurar acesso externo:

        sudo ufw allow 11434/tcp
        sudo ufw reload

    - **Opcional** A operação anterior pode causar erro de permissão no docker, nesse caso rode o domando:

        sudo chmod 666 /var/run/docker.sock

## Subindo projeto e setup do chatbot

    - Iniciar containers

        docker compose create && docker compose --profile cpu up

    - Acessar N8N pelo navegador na porta configurada (padrão:5678)
    - Criar credenciais acesso (emnail, nome de usuário, senha)
    - Clicar 'Start from scratch' para dar início ao seu primeiro workflow:



    - O passo anterior deve ter te levado a aba do editor. Clique em 'Add first step' para criar um trigger. Nesse caso, escolheremos 'On chat message':



    - Com o trigger criado, devemos adicionar uma função. Aqui, deveremos selecionar 'Advanced AI' e, então 'AI agent'






    - Agora é necessário configurar o AI Agent, a começar pelo modelo. Clique no sinal de "+" indicado por "chat model" e escolha seu modelo. Nesse caso selecionaremos o Ollama:



    - Você deve ser encaminhado para a página de configuração do Ollama. Comece criando uma nova creddencial:



    - No campo "Base URL", configure a conexão diretamente com a sua instância local na porta 11434:

        http://<ip_da_dua_maquina>:11434

    Após salvar a credencial, você deve receber uma mensagem de sucesso:



    - Se a conexão for bem sucedida, o campo 'model' de configuração do ollama deve exibir um dropdown com os modelos de IA que estão salvos no seu Ollama local. Selecione seu modelo de preferência:



    - Com essa configuração básica, é possível ter interações simples com o modelo atrvés do chat




