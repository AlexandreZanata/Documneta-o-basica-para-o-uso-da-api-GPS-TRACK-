# smartGPS Tracking Software API

Bem-vindo ao guia passo-a-passo para utilizar a **API Tracking Software** do smartGPS. Este README foi elaborado de forma extremamente didática para que qualquer pessoa, mesmo sem experiência prévia, consiga configurar, autenticar e consumir os principais recursos da API.

---

## 📘 Visão Geral

A API Tracking Software permite o envio e consulta de dados de rastreamento em tempo real. Você poderá:

* Enviar informações de localização, velocidade e status do dispositivo.
* Consultar históricos de rastreamento.
* Aplicar filtros de data, dispositivo e paginação.

---

## 🛠️ Pré-requisitos

Antes de começar, certifique-se de ter:

1. Uma **conta** no portal smartGPS.
2. Acesso à **chave de API** (API Key) ou **token de autenticação**.
3. Ferramenta para fazer requisições HTTP (cURL, Postman, Insomnia) ou linguagem de programação com biblioteca HTTP.
4. Noções básicas de JSON e linhas de comando.

---

## 💻 Configuração do Ambiente Local

Antes de testar a API no seu computador, siga este passo a passo para preparar as ferramentas necessárias:

1. **Terminal de Comandos**

   * **Windows:** use o PowerShell ou Git Bash (instalado com o Git).
   * **macOS/Linux:** utilize o Terminal padrão.

2. **cURL** (linha de comando para HTTP)

   * **Windows 10+**: já disponível no PowerShell.
   * **macOS:** instalado por padrão.
   * **Linux:** instale com `sudo apt install curl` ou equivalente.

3. **Ferramenta Gráfica de API (opcional)**

   * **Postman**: [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
   * **Insomnia**: [https://insomnia.rest/download](https://insomnia.rest/download)

4. **Node.js (v14+ recomendado)**

   * Baixe o instalador em [https://nodejs.org](https://nodejs.org) e siga as instruções.
   * Verifique a instalação:

     ```bash
     node -v
     npm -v
     ```

5. **Python (v3.7+ recomendado)**

   * Baixe de [https://www.python.org/downloads/](https://www.python.org/downloads/)
   * Verifique:

     ```bash
     python --version
     # ou
     python3 --version
     ```

6. **(Opcional) Projeto de Teste em Node.js**

   ```bash
   mkdir smartgps-test && cd smartgps-test
   npm init -y
   npm install axios dotenv
   ```

7. **Variáveis de Ambiente**

   * Crie um arquivo `.env` na raiz do projeto com:

     ```dotenv
     BASE_URL=https://sandbox.smartgps.com.br
     TOKEN=SEU_ACCESS_TOKEN
     DEVICE_ID=TRACKER_1234
     ```

8. **Script de Teste (test.js)**

   ```js
   require('dotenv').config();
   const axios = require('axios');

   // Exemplo de payload para envio de rastreamento
   const payload = {
     deviceId: process.env.DEVICE_ID,
     timestamp: new Date().toISOString(),
     location: { latitude: -15.5989, longitude: -56.0974, altitude: 350.5 },
     speed: 72.3,
     heading: 245.0,
     satellites: 8,
     battery: 87
   };

   async function sendTracking() {
     try {
       const res = await axios.post(
         `${process.env.BASE_URL}/v1/tracking`,
         payload,
         { headers: { Authorization: `Bearer ${process.env.TOKEN}` } }
       );
       console.log('Resposta:', res.data);
     } catch (err) {
       console.error('Erro:', err.response ? err.response.data : err.message);
     }
   }

   sendTracking();
   ```

9. **Executar o Teste**

   ```bash
   node test.js
   ```

---

## 🔗 Endpoints Principais

| Método | Endpoint         | Descrição                                   |
| ------ | ---------------- | ------------------------------------------- |
| `POST` | `/v1/auth/login` | Autenticação e obtenção do token Bearer     |
| `POST` | `/v1/tracking`   | Envio de dados de rastreamento              |
| `GET`  | `/v1/tracking`   | Listagem de dados de rastreamento (filtros) |

> **Obs.:** O Base URL padrão é `https://api.smartgps.com.br` (Produção) e `https://sandbox.smartgps.com.br` (Homologação).

---

## 📝 Estrutura de Autenticação

### 1. Obtenção do Token

Faça uma requisição `POST` para o endpoint `/v1/auth/login` com as suas credenciais:

```bash
overview
POST {{BASE_URL}}/v1/auth/login
Content-Type: application/json

{
  "username": "SEU_USUARIO",
  "password": "SUA_SENHA"
}
```

> **Parâmetros**:
>
> * `username`: seu usuário no portal.
> * `password`: sua senha.

### 2. Exemplo de Resposta

```json
{
  "access_token": "eyJhbGciOiJI...",
  "expires_in": 3600
}
```

> Armazene o `access_token`. Nas próximas chamadas, inclua no header:
>
> ```http
> Authorization: Bearer SEU_ACCESS_TOKEN
> ```

---

## 🚀 Enviando Dados de Rastreamento

### 1. Formato do Payload

```json
{
  "deviceId": "TRACKER_1234",
  "timestamp": "2025-07-14T13:45:30Z",
  "location": {
    "latitude": -15.5989,
    "longitude": -56.0974,
    "altitude": 350.5
  },
  "speed": 72.3,
  "heading": 245.0,
  "satellites": 8,
  "battery": 87
}
```

| Campo        | Tipo    | Descrição                             |
| ------------ | ------- | ------------------------------------- |
| `deviceId`   | string  | Identificador único do dispositivo    |
| `timestamp`  | string  | Data e hora no formato ISO 8601 (UTC) |
| `location`   | object  | Objeto com coordenadas e altitude     |
| `latitude`   | float   | Latitude em graus decimais            |
| `longitude`  | float   | Longitude em graus decimais           |
| `altitude`   | float   | Altitude em metros                    |
| `speed`      | float   | Velocidade em km/h                    |
| `heading`    | float   | Rumo em graus (0–359)                 |
| `satellites` | integer | Número de satélites usados            |
| `battery`    | integer | Nível de bateria em porcentagem       |

### 2. Exemplo de Requisição cURL

```bash
overview
curl -X POST {{BASE_URL}}/v1/tracking \
  -H "Authorization: Bearer SEU_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '@payload.json'
```

### 3. Exemplo de Resposta

```json
{
  "status": "success",
  "message": "Data received",
  "dataId": 987654,
  "receivedAt": "2025-07-14T13:45:31Z"
}
```

---

## 🔍 Consultando Dados de Rastreamento

### 1. Parâmetros de Filtro

* `deviceId` (string): filtro por dispositivo.
* `limit` (integer): número máximo de registros retornados.
* `offset` (integer): deslocamento para paginação.
* `dateFrom` e `dateTo` (ISO 8601): intervalo de datas.

### 2. Exemplo de Requisição

```bash
overview
GET {{BASE_URL}}/v1/tracking?deviceId=TRACKER_1234&limit=50&dateFrom=2025-07-14T00:00:00Z&dateTo=2025-07-14T23:59:59Z
Authorization: Bearer SEU_ACCESS_TOKEN
```

### 3. Exemplo de Resposta

```json
[
  {
    "dataId": 987654,
    "deviceId": "TRACKER_1234",
    "timestamp": "2025-07-14T13:45:30Z",
    "location": {"latitude": -15.5989, "longitude": -56.0974},
    "speed": 72.3
  },
  {
    "dataId": 987653,
    "deviceId": "TRACKER_1234",
    "timestamp": "2025-07-14T13:40:15Z",
    "location": {"latitude": -15.5975, "longitude": -56.0952},
    "speed": 68.1
  }
]
```

---

## 📚 Exemplos de Código

### JavaScript (fetch)

```js
async function sendTracking(data, token) {
  const response = await fetch(`${BASE_URL}/v1/tracking`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
  });
  if (!response.ok) throw new Error('Erro ao enviar dados');
  return response.json();
}
```

### Python (requests)

```python
import requests

def send_tracking(data, token):
    url = f"{BASE_URL}/v1/tracking"
    headers = {
        'Authorization': f'Bearer {token}',
        'Content-Type': 'application/json'
    }
    resp = requests.post(url, json=data, headers=headers)
    resp.raise_for_status()
    return resp.json()
```

---

## ✅ Melhores Práticas e Validações

1. **Campos Obrigatórios**: `deviceId`, `timestamp`, `latitude`, `longitude`.
2. **Formato ISO 8601**: sempre use UTC (ex.: `2025-07-14T13:45:30Z`).
3. **Faixas Válidas**:

   * Latitude: –90 a +90
   * Longitude: –180 a +180
   * Velocidade: 0 a 300 km/h (ajuste conforme seu dispositivo)
4. **Tratamento de Erros**:

   * Verifique códigos 4xx/5xx e implemente retry/backoff.

---

## 🌟 Próximos Passos Avançados

* **Webhooks**: receba notificações em tempo real.
* **Mapeamento de Rotas**: visualize dados em mapas (Leaflet, Google Maps).
* **Alertas**: configure limites de velocidade ou áreas geográficas.
* **Página de Dashboard**: crie UI para acompanhar sua frota.


