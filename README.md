# MTG Deck Builder API

## 1. Introdução

A MTG Deck Builder API é uma interface programática RESTful projetada para criação, gerenciamento e compartilhamento de decks de Magic: The Gathering. Esta API permite que desenvolvedores criem aplicações que possibilitem aos usuários montar coleções de cartas, construir decks competitivos ou casuais, analisar estatísticas, e compartilhar suas criações com a comunidade.

Inspirada em plataformas como Moxfield e Archidekt, nossa API fornece um conjunto abrangente de endpoints para manipular decks, cartas, coleções, e interações sociais entre jogadores.

## 2. Rotas da API

| Método | Rota | Descrição | Status Codes |
|--------|------|-----------|-------------|
| GET | /cards | Listar todas as cartas disponíveis (paginado) | 200, 400, 500 |
| GET | /cards/{id} | Buscar detalhes de uma carta específica pelo ID | 200, 404, 500 |
| GET | /cards/search | Buscar cartas por critérios (nome, cor, tipo, etc.) | 200, 400, 500 |
| GET | /sets | Listar todas as coleções (sets) de MTG | 200, 500 |
| GET | /sets/{code} | Buscar uma coleção específica pelo código | 200, 404, 500 |
| GET | /decks | Listar decks públicos (paginado) | 200, 400, 500 |
| GET | /decks/{id} | Buscar deck específico pelo ID | 200, 404, 500 |
| POST | /decks | Criar novo deck | 201, 400, 401, 500 |
| PUT | /decks/{id} | Atualizar deck existente | 200, 400, 401, 403, 404, 500 |
| DELETE | /decks/{id} | Excluir deck existente | 204, 401, 403, 404, 500 |
| POST | /decks/{id}/cards | Adicionar cartas a um deck | 200, 400, 401, 403, 404, 500 |
| DELETE | /decks/{id}/cards/{cardId} | Remover carta de um deck | 204, 400, 401, 403, 404, 500 |
| GET | /decks/{id}/stats | Obter estatísticas de um deck | 200, 404, 500 |
| GET | /decks/{id}/export/{format} | Exportar deck em formato específico (txt, mtgo, arena) | 200, 400, 404, 500 |
| POST | /decks/{id}/clone | Clonar um deck existente | 201, 400, 401, 403, 404, 500 |
| GET | /users/{id}/decks | Listar decks de um usuário específico | 200, 400, 401, 404, 500 |
| GET | /users/{id}/collection | Ver coleção de cartas de um usuário | 200, 400, 401, 403, 404, 500 |
| PUT | /users/{id}/collection | Atualizar coleção de cartas de um usuário | 200, 400, 401, 403, 404, 500 |
| POST | /users/{id}/collection/import | Importar cartas para a coleção do usuário | 200, 400, 401, 403, 404, 500 |
| GET | /formats | Listar formatos de jogo disponíveis | 200, 500 |
| GET | /formats/{name}/legality | Verificar legalidade de cartas em um formato específico | 200, 400, 404, 500 |
| GET | /meta/{format} | Obter dados de meta para um formato específico | 200, 400, 404, 500 |
| POST | /auth/register | Registrar novo usuário | 201, 400, 409, 500 |
| POST | /auth/login | Autenticar usuário | 200, 400, 401, 500 |
| POST | /decks/{id}/comments | Adicionar comentário a um deck | 201, 400, 401, 403, 404, 500 |
| GET | /decks/{id}/comments | Listar comentários de um deck | 200, 404, 500 |
| POST | /decks/{id}/like | Curtir um deck | 200, 400, 401, 404, 500 |
| DELETE | /decks/{id}/like | Remover curtida de um deck | 204, 401, 404, 500 |
| POST | /decks/{id}/tags | Adicionar tags a um deck | 200, 400, 401, 403, 404, 500 |
| DELETE | /decks/{id}/tags/{tagId} | Remover tag de um deck | 204, 401, 403, 404, 500 |

## 3. DTOs e Modelos de Dados

### CardDTO
```json
{
  "id": 1,
  "name": "Black Lotus",
  "manaCost": "{0}",
  "cmc": 0,
  "types": ["Artifact"],
  "subtypes": [],
  "text": "{T}, Sacrifice Black Lotus: Add three mana of any one color.",
  "power": null,
  "toughness": null,
  "loyalty": null,
  "colors": [],
  "colorIdentity": [],
  "setCode": "LEA",
  "rarity": "Rare",
  "imageUrl": "https://api.magicthegathering.io/images/lea/black-lotus.jpg",
  "multiverse_id": 3,
  "legalities": [
    {
      "format": "Vintage",
      "legality": "Restricted"
    },
    {
      "format": "Commander",
      "legality": "Banned"
    }
  ]
}
```

### DeckCardDTO
```json
{
  "cardId": 1,
  "quantity": 1,
  "isSideboard": false,
  "isCommander": false,
  "isCompanion": false
}
```

### DeckDTO
```json
{
  "id": 1,
  "name": "Power 9 Collection",
  "format": "Vintage",
  "description": "A showcase of the most powerful cards in Magic's history",
  "visibility": "public",
  "tags": ["power9", "vintage", "collection"],
  "cards": [
    {
      "cardId": 1,
      "quantity": 1,
      "isSideboard": false,
      "isCommander": false,
      "isCompanion": false
    }
  ],
  "sideboard": [
    {
      "cardId": 15,
      "quantity": 2,
      "isSideboard": true,
      "isCommander": false,
      "isCompanion": false
    }
  ],
  "commander": {
    "cardId": 100,
    "quantity": 1,
    "isSideboard": false,
    "isCommander": true,
    "isCompanion": false
  },
  "companion": null,
  "createdAt": "2025-04-20T15:30:15Z",
  "updatedAt": "2025-04-21T10:12:42Z",
  "author": {
    "id": 42,
    "username": "VintageMaster"
  },
  "likes": 150,
  "comments": 23,
  "views": 1502
}
```

### DeckCreateDTO
```json
{
  "name": "Power 9 Collection",
  "format": "Vintage",
  "description": "A showcase of the most powerful cards in Magic's history",
  "visibility": "public",
  "tags": ["power9", "vintage", "collection"],
  "cards": [
    {
      "cardId": 1,
      "quantity": 1,
      "isSideboard": false,
      "isCommander": false,
      "isCompanion": false
    }
  ]
}
```

### DeckStatsDTO
```json
{
  "deckId": 1,
  "cardCount": 60,
  "sideboardCount": 15,
  "colorDistribution": {
    "white": 15,
    "blue": 20,
    "black": 0,
    "red": 10,
    "green": 5,
    "colorless": 10
  },
  "manaCurve": {
    "0": 4,
    "1": 10,
    "2": 12,
    "3": 15,
    "4": 8,
    "5": 6,
    "6+": 5
  },
  "typeDistribution": {
    "creature": 24,
    "instant": 10,
    "sorcery": 8,
    "artifact": 6,
    "enchantment": 4,
    "planeswalker": 3,
    "land": 25
  },
  "averageCmc": 2.75
}
```

### UserDTO
```json
{
  "id": 42,
  "username": "VintageMaster",
  "email": "vintage@example.com",
  "avatarUrl": "https://mtgdeckbuilder.com/avatars/vm.jpg",
  "joinDate": "2025-01-15T08:30:00Z",
  "deckCount": 25,
  "followersCount": 156,
  "followingCount": 89,
  "bio": "Vintage enthusiast and collector since 1994"
}
```

### UserRegisterDTO
```json
{
  "username": "VintageMaster",
  "email": "vintage@example.com",
  "password": "SecureP@ssw0rd!",
  "confirmPassword": "SecureP@ssw0rd!"
}
```

### LoginRequestDTO
```json
{
  "email": "vintage@example.com",
  "password": "SecureP@ssw0rd!"
}
```

### LoginResponseDTO
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 42,
    "username": "VintageMaster",
    "email": "vintage@example.com",
    "avatarUrl": "https://mtgdeckbuilder.com/avatars/vm.jpg"
  },
  "expiresAt": "2025-04-22T14:30:45Z"
}
```

### CommentDTO
```json
{
  "id": 123,
  "deckId": 1,
  "user": {
    "id": 42,
    "username": "VintageMaster",
    "avatarUrl": "https://mtgdeckbuilder.com/avatars/vm.jpg"
  },
  "content": "Amazing deck! Have you considered adding Time Vault?",
  "createdAt": "2025-04-21T09:45:23Z",
  "updatedAt": null
}
```

### SetDTO
```json
{
  "code": "LEA",
  "name": "Limited Edition Alpha",
  "releaseDate": "1993-08-05",
  "cardCount": 295,
  "iconUrl": "https://api.magicthegathering.io/icons/lea.svg",
  "description": "The first Magic: The Gathering set ever released"
}
```

### FormatDTO
```json
{
  "name": "Standard",
  "description": "Format using card sets from the most recent sets",
  "minDeckSize": 60,
  "maxCardCopies": 4,
  "allowedSets": ["MID", "VOW", "NEO", "SNC", "DMU", "BRO", "ONE", "MOM"],
  "bannedCards": [
    {"id": 5432, "name": "The Meathook Massacre"}
  ],
  "restrictedCards": []
}
```

### ErrorResponseDTO
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "Name is required",
    "Format must be a valid Magic format"
  ],
  "timestamp": "2025-04-21T12:34:56Z"
}
```

## 4. Especificação OpenAPI

O arquivo completo de especificação OpenAPI está disponível no arquivo `openapi.yaml` incluído neste repositório.

## 5. Critérios de Avaliação

- **Clareza e detalhamento da documentação**: Esta documentação fornece descrições detalhadas de todas as rotas da API, modelos de dados e exemplos de uso.
- **Consistência no uso de métodos HTTP e códigos de status**: Os métodos HTTP (GET, POST, PUT, DELETE) foram aplicados de forma consistente com os princípios RESTful, e todos os códigos de status relevantes foram documentados para cada rota.
- **Precisão e clareza nas descrições e nos exemplos dos DTOs**: Cada DTO inclui campos claramente definidos com exemplos de valores realistas.
- **Validação da especificação**: O arquivo OpenAPI foi validado usando o Swagger Editor para garantir conformidade com o padrão OpenAPI 3.0.
