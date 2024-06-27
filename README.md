# Utilizando a Microsoft Graph API

Este projeto demonstra como utilizar a Microsoft Graph API para acessar dados do Microsoft 365. O script utiliza a biblioteca `requests` para fazer chamadas HTTP e `azure.identity.InteractiveBrowserCredential` para autenticação.

## Pré-requisitos

- Python 3.6 < 3.x
- Uma conta Microsoft 365

## Instalação

1. Clone este repositório:

```bash
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio
```

2. Crie e ative um ambiente virtual:

```bash
python -m venv venv
source venv/bin/activate  #No Windows, use `venv\Scripts\activate`
```

3. Instale as dependências:

```bash
pip install -r requirements.txt
```

## Como Utilizar:

### Importações Necessárias

```python
import requests
from azure.identity import InteractiveBrowserCredential
```

### Funções Utilitárias

```python
# Obter token de acesso usando azure-identity
def get_access_token():
    credentials = InteractiveBrowserCredential()
    token = credentials.get_token("https://graph.microsoft.com/.default")
    return token.token

# Obter o ID do site
def get_site_id(access_token):
    url = 'https://graph.microsoft.com/v1.0/sites/root'
    headers = {
        'Authorization': f'Bearer {access_token}',
        'Content-Type': 'application/json'
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        return response.json()['id']
    else:
        print(f"Erro ao obter site ID: {response.status_code}")
        return None

# Função principal
def main():
    access_token = get_access_token()
    site_id = get_site_id(access_token)
    
    if site_id:
        print("Site ID:", site_id)
    else:
        print("Erro ao obter site ID")

if __name__ == "__main__":
    main()
```

### Executando o Script

Para executar o script:

```bash
python seu_script.py
```

Este script realiza a autenticação usando `azure.identity` e obtém o ID do site raiz do Microsoft 365. Certifique-se de estar autenticado e ter as permissões necessárias para acessar a Graph API antes de executar o script.
