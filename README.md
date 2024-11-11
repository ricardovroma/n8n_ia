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

        ![workflow_tutorial_0](https://github.com/user-attachments/assets/2586aca6-7126-4e2d-aa66-c826f88f1141)

    - O passo anterior deve ter te levado a aba do editor. Clique em 'Add first step' para criar um trigger. Nesse caso, escolheremos 'On chat message':

        ![workflow_tutorial_1](https://github.com/user-attachments/assets/78852d7c-7466-45c3-8ea6-055f3b5ffdae)

    - Com o trigger criado, devemos adicionar uma função. Aqui, deveremos selecionar 'Advanced AI' e, então 'AI agent'

        ![workflow_tutorial_2](https://github.com/user-attachments/assets/00f8834a-ac95-47bb-806f-2f4ac2004f75)
    
        ![workflow_tutorial_3](https://github.com/user-attachments/assets/53d13ab4-62e7-4fd4-ac0e-1e587938c12d)

    - Agora é necessário configurar o AI Agent, a começar pelo modelo. Clique no sinal de "+" indicado por "chat model" e escolha seu modelo. Nesse caso             selecionaremos o Ollama:

        ![workflow_tutorial_4](https://github.com/user-attachments/assets/cbe00839-8cc4-4afd-a6be-526bff30e43d)

    - Você deve ser encaminhado para a página de configuração do Ollama. Comece criando uma nova creddencial:

    ![workflow_tutorial_6](https://github.com/user-attachments/assets/a7104dce-6e76-4785-a857-c8e5585d0e19)

    - No campo "Base URL", configure a conexão diretamente com a sua instância local na porta 11434:

        http://<ip_da_dua_maquina>:11434

    Após salvar a credencial, você deve receber uma mensagem de sucesso:

        ![workflow_tutorial_7](https://github.com/user-attachments/assets/6b81d8aa-aa2f-4836-9d28-3e9acb18a5a8)

    - Se a conexão for bem sucedida, o campo 'model' de configuração do ollama deve exibir um dropdown com os modelos de IA que estão salvos no seu Ollama             local. Selecione seu modelo de preferência:

        ![workflow_tuttorial_8](https://github.com/user-attachments/assets/f50cfc92-759f-4070-bd02-5ef27de775ab)

    - Com essa configuração básica, é possível ter interações simples com o modelo atrvés do chat

        ![workflow_tutorial_10](https://github.com/user-attachments/assets/9f80c44f-2aba-4266-b1c9-941012097c9c)



