<script>
import axios from 'axios'

export default {
  name: 'App',

  data() {
    return {
      contatos: [],
      carregando: true,
      mensagemErro: ''
    }
  },

  async mounted() {
    try {
      const resposta = await axios.get(
        'https://jsonplaceholder.typicode.com/users'
      )

      this.contatos = resposta.data
    } catch (erro) {
      console.error('Erro ao carregar os contatos:', erro)
      this.mensagemErro = 'Não foi possível carregar os contatos.'
    } finally {
      this.carregando = false
    }
  }
}
</script>

<template>
  <main class="container">
    <header>
      <p class="subtitulo">Diretório empresarial</p>
      <h1>Contatos Corporativos</h1>
      <p>Consulte as informações dos colaboradores da empresa.</p>
    </header>

    <p v-if="carregando" class="aviso">
      Carregando contatos...
    </p>

    <p v-else-if="mensagemErro" class="erro">
      {{ mensagemErro }}
    </p>

    <section v-else class="lista-contatos">
      <article
        v-for="contato in contatos"
        :key="contato.id"
        class="cartao"
      >
        <div class="avatar">
          {{ contato.name.charAt(0) }}
        </div>

        <div class="informacoes">
          <h2>{{ contato.name }}</h2>
          <p class="empresa">{{ contato.company.name }}</p>

          <p>
            <strong>E-mail:</strong>
            {{ contato.email }}
          </p>

          <p>
            <strong>Telefone:</strong>
            {{ contato.phone }}
          </p>

          <p>
            <strong>Cidade:</strong>
            {{ contato.address.city }}
          </p>

          <a
            :href="'https://' + contato.website"
            target="_blank"
          >
            Visitar site
          </a>
        </div>
      </article>
    </section>
  </main>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

.container {
  width: 100%;
  min-height: 100vh;
  padding: 50px 8%;
  background: #f4f7fb;
  font-family: Arial, sans-serif;
  color: #1f2937;
}

header {
  margin-bottom: 35px;
}

header h1 {
  margin: 5px 0 10px;
  font-size: 36px;
  color: #172554;
}

header p {
  color: #64748b;
}

.subtitulo {
  margin: 0;
  color: #2563eb;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.lista-contatos {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(290px, 1fr));
  gap: 22px;
}

.cartao {
  display: flex;
  gap: 18px;
  padding: 24px;
  border: 1px solid #e2e8f0;
  border-radius: 16px;
  background: white;
  box-shadow: 0 8px 20px rgba(15, 23, 42, 0.06);
  transition: transform 0.2s;
}

.cartao:hover {
  transform: translateY(-4px);
}

.avatar {
  display: flex;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  width: 55px;
  height: 55px;
  border-radius: 50%;
  background: #2563eb;
  color: white;
  font-size: 24px;
  font-weight: bold;
}

.informacoes h2 {
  margin: 0 0 5px;
  font-size: 20px;
}

.informacoes p {
  margin: 8px 0;
  color: #475569;
}

.informacoes .empresa {
  margin-bottom: 16px;
  color: #2563eb;
  font-weight: bold;
}

.informacoes a {
  display: inline-block;
  margin-top: 10px;
  color: #2563eb;
  font-weight: bold;
  text-decoration: none;
}

.aviso,
.erro {
  padding: 20px;
  border-radius: 10px;
  background: white;
  text-align: center;
}

.erro {
  color: #b91c1c;
}
</style>