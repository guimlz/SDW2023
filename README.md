# SDW2023

Contexto: Você é um cientista de dados no Santander e recebeu a tarefa de envolver seus clientes de maneira mais personalizada. Seu objetivo é usar o poder da IA Generativa para criar mensagens de marketing personalizadas que serão entregues a cada cliente.

```ruby
sdw2023_api_url = 'https://sdw-2023-prd.up.railway.app'
```

## Extract
```ruby

import pandas as pd

df = pd.read_csv('SDW2023.csv.txt')
user_ids = df['UserID'].tolist()
print(user_ids)

import requests
import json

def get_user(id):
  response = requests.get(f'https://sdw-2023-prd.up.railway.app/users/{id}')
  return response.json() if response.status_code == 200 else None

users = [user for id in user_ids if (user := get_user(id)) is not None]
print(json.dumps(users, indent=2))
```

## Transform
```ruby

!pip install openai

openai_api_key = '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'

import openai

openai.api_key = openai_api_key

def generate_ai_news(user):
  completion = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
      {
          "role": "system",
          "content": "Você é um especialista em markting bancário."
      },
      {
          "role": "user",
          "content": f"Crie uma mensagem para {user['name']} sobre a importância dos investimentos (máximo de 100 caracteres)"
      }
    ]
  )
  return completion.choices[0].message.content.strip('\"')

for user in users:
  news = generate_ai_news(user)
  print(news)
  user['news'].append({
      "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
      "description": news
  })
```

## Load

```ruby
[ ]
def update_user(user):
  response = requests.put(f"{sdw2023_api_url}/users/{user['id']}", json=user)
  return True if response.status_code == 200 else False

for user in users:
  success = update_user(user)
  print(f"User {user['name']} updated? {success}!")
```

## Resultados
```ruby
[
  {
    "id": 5233,
    "name": "guimlz",
    "account": {
      "id": 5562,
      "number": "49146761845",
      "agency": "1234",
      "balance": 0.0,
      "limit": 1000.0
    },
    "card": {
      "id": 5090,
      "number": "49146761845",
      "limit": 1000.0
    },
    "features": [
      {
        "id": 1605,
        "icon": "string",
        "description": "string"
      }
    ],
    "news": [
      {
        "id": 9642,
        "icon": "string",
        "description": ""Cada cliente é especial. Seu sucesso financeiro é nossa missão. Conte com soluções personalizadas para você. #InovaçãoBancária""
      }
    ]'

{
  "id": 5234,
  "name": "nini",
  "account": {
    "id": 5563,
    "number": "539835274",
    "agency": "1234",
    "balance": 0,
    "limit": 1000
  },
  "card": {
    "id": 5091,
    "number": "539835274",
    "limit": 1000
  },
  "features": [
    {
      "id": 1606,
      "icon": "string",
      "description": "string"
    }
  ],
  "news": [
    {
      "id": 9643,
      "icon": "string",
      "description": "Descubra um mundo de possibilidades bancárias personalizadas para você. Sua jornada financeira, nossa prioridade. #InovaçãoBancária"
    }
  ]
}
{
  "id": 5235,
  "name": "pmiguel",
  "account": {
    "id": 5564,
    "number": "21527955800",
    "agency": "1234",
    "balance": 0,
    "limit": 1000
  },
  "card": {
    "id": 5092,
    "number": "21527955800",
    "limit": 1000
  },
  "features": [
    {
      "id": 1607,
      "icon": "string",
      "description": "string"
    }
  ],
  "news": [
    {
      "id": 9644,
      "icon": "string",
      "description": "Sua prosperidade é nossa prioridade. Descubra ofertas personalizadas feitas sob medida para você. #InovaçãoFinanceira"
    }
  ]
}
```
