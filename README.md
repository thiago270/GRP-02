#Aluno _

*Thiago Luiz De Souza Santos- 01644540


const express = require('express');
const https = require('https');

const app = express();
const PORT = 3000;

app.get('/pokemon', (req, res) => {
 
  https.get('https://pokeapi.co/api/v2/pokemon/1', (apiRes) => {
    let data = '';

    apiRes.on('data', (chunk) => {
      data += chunk;
    });

    apiRes.on('end', () => {
      try {
        const pokemonData = JSON.parse(data);

        // Calcula o somatório das habilidades
        const abilitiesSum = pokemonData.abilities.reduce(
          (sum, ability) => sum + ability.slot,
          0
        );

        // Constrói o objeto JSON conforme especificado
        const responseObject = {
          abilities: abilitiesSum,
          name: pokemonData.name,
          back_default: pokemonData.sprites.back_default,
        };

        // Envia o objeto JSON como resposta
        res.json(responseObject);
      } catch (error) {
        console.error('Erro ao analisar os dados da API:', error);
        res.status(500).send('Erro interno do servidor');
      }
    });
  }).on('error', (error) => {
    console.error('Erro na requisição à API do Pokémon:', error);
    res.status(500).send('Erro interno do servidor');
  });
});

// Inicia o servidor Express
app.listen(PORT, () => {
  console.log(`Servidor rodando em http://localhost:$3000`);
});
