<template>
  <div class="travel-page">
    <!-- Header da página -->
    <div class="page-header">
      <h1>{{ agencyName }}</h1>
      <p class="subtitle">{{ agencySlogan }}</p>
    </div>

    <!-- Seção de boas-vindas -->
    <div class="welcome-section">
      <h2>Bem-vindo, {{ customerName || 'Visitante' }}!</h2>
      <p>{{ welcomeMessage }}</p>
      <p class="current-date">Data de hoje: {{ formattedDate }}</p>
    </div>

    <!-- Seção de destinos -->
    <div class="destinations-section">
      <h3>Destinos em Destaque</h3>
      <div class="destinations-grid">
        <div 
          v-for="destination in destinations" 
          :key="destination.id"
          class="destination-card"
        >
          <img :src="destination.image" :alt="destination.name" />
          <div class="card-content">
            <h4>{{ destination.name }}</h4>
            <p>{{ destination.description }}</p>
            <div class="price-info">
              <span class="price">R$ {{ formatPrice(destination.price) }}</span>
              <span class="duration">{{ destination.duration }} dias</span>
            </div>
            <button class="btn-primary" @click="selectDestination(destination)">
              Saiba mais
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Seção de estatísticas -->
    <div class="stats-section">
      <h3>{{ statsTitle }}</h3>
      <div class="stats-grid">
        <div class="stat-item">
          <h4>{{ totalCustomers }}</h4>
          <p>Clientes Satisfeitos</p>
        </div>
        <div class="stat-item">
          <h4>{{ totalDestinations }}</h4>
          <p>Destinos Disponíveis</p>
        </div>
        <div class="stat-item">
          <h4>{{ yearsInBusiness }}</h4>
          <p>Anos de Experiência</p>
        </div>
      </div>
    </div>

    <!-- Seção de contato -->
    <div class="contact-section">
      <h3>Entre em Contato</h3>
      <div class="contact-info">
        <p><strong>Email:</strong> {{ contactInfo.email }}</p>
        <p><strong>Telefone:</strong> {{ contactInfo.phone }}</p>
        <p><strong>Endereço:</strong> {{ contactInfo.address }}</p>
      </div>
      
      <!-- Formulário dinâmico -->
      <div class="contact-form">
        <h4>Envie sua mensagem</h4>
        <form @submit.prevent="submitForm">
          <input 
            v-model="form.name" 
            type="text" 
            :placeholder="`Digite seu nome`"
            required 
          />
          <input 
            v-model="form.email" 
            type="email" 
            :placeholder="`Digite seu email`"
            required 
          />
          <textarea 
            v-model="form.message" 
            :placeholder="`Conte-nos sobre sua viagem dos sonhos...`"
            rows="4"
            required
          ></textarea>
          <button type="submit" class="btn-primary">
            {{ isSubmitting ? 'Enviando...' : 'Enviar Mensagem' }}
          </button>
        </form>
        
        <!-- Mensagem de status -->
        <div v-if="statusMessage" :class="messageClass">
          {{ statusMessage }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'

export default {
  name: 'TravelAgencyPage',
  setup() {
    // Variáveis reativas
    const agencyName = ref('Agência de Viagem')
    const agencySlogan = ref('Descubra destinos incríveis com a gente ✈️')
    const customerName = ref('')
    const welcomeMessage = ref('Explore o mundo com nossos pacotes exclusivos!')
    const statsTitle = ref('Nossos Números')
    const isSubmitting = ref(false)
    const statusMessage = ref('')
    const messageType = ref('') // 'success' ou 'error'
    
    // Informações de contato
    const contactInfo = ref({
      email: 'contato@agenciaviagem.com.br',
      phone: '(11) 9999-8888',
      address: 'Rua das Viagens, 123 - São Paulo, SP'
    })

    // Formulário
    const form = ref({
      name: '',
      email: '',
      message: ''
    })

    // Estatísticas
    const totalCustomers = ref(1250)
    const totalDestinations = ref(45)
    const yearsInBusiness = ref(12)

    // Destinos
    const destinations = ref([
      {
        id: 1,
        name: 'Praias do Caribe',
        description: 'Águas cristalinas e areias brancas te esperam no paraíso caribenho.',
        price: 3500.00,
        duration: 7,
        image: '/images/caribe.jpg'
      },
      {
        id: 2,
        name: 'Montanhas da Suíça',
        description: 'Paisagens alpinas deslumbrantes e ar puro da montanha.',
        price: 4200.00,
        duration: 10,
        image: '/images/suica.jpg'
      },
      {
        id: 3,
        name: 'Cidades Históricas',
        description: 'Explore a rica história e arquitetura européia.',
        price: 2800.00,
        duration: 8,
        image: '/images/historicas.jpg'
      }
    ])

    // Computed properties
    const formattedDate = computed(() => {
      return new Date().toLocaleDateString('pt-BR', {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      })
    })

    const messageClass = computed(() => {
      return messageType.value === 'success' ? 'message success' : 'message error'
    })

    // Métodos
    const formatPrice = (price) => {
      return price.toLocaleString('pt-BR', {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      })
    }

    const selectDestination = (destination) => {
      alert(`Você selecionou: ${destination.name}\nPreço: R$ ${formatPrice(destination.price)}\nDuração: ${destination.duration} dias`)
    }

    const submitForm = async () => {
      isSubmitting.value = true
      statusMessage.value = ''
      
      try {
        // Simula envio do formulário
        await new Promise(resolve => setTimeout(resolve, 2000))
        
        statusMessage.value = `Obrigado ${form.value.name}! Sua mensagem foi enviada com sucesso. Entraremos em contato em breve.`
        messageType.value = 'success'
        
        // Limpa o formulário
        form.value = { name: '', email: '', message: '' }
      } catch (error) {
        statusMessage.value = 'Erro ao enviar mensagem. Tente novamente.'
        messageType.value = 'error'
      } finally {
        isSubmitting.value = false
        
        // Remove a mensagem após 5 segundos
        setTimeout(() => {
          statusMessage.value = ''
        }, 5000)
      }
    }

    // Lifecycle
    onMounted(() => {
      // Simula carregar nome do usuário (poderia vir de uma API)
      setTimeout(() => {
        customerName.value = 'João Silva' // Exemplo
      }, 1000)
    })

    return {
      // Variáveis reativas
      agencyName,
      agencySlogan,
      customerName,
      welcomeMessage,
      statsTitle,
      isSubmitting,
      statusMessage,
      contactInfo,
      form,
      totalCustomers,
      totalDestinations,
      yearsInBusiness,
      destinations,
      
      // Computed
      formattedDate,
      messageClass,
      
      // Métodos
      formatPrice,
      selectDestination,
      submitForm
    }
  }
}
</script>

<style scoped>
.travel-page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Arial', sans-serif;
}

.page-header {
  text-align: center;
  background: linear-gradient(135deg, #4a90e2, #357abd);
  color: white;
  padding: 60px 20px;
  border-radius: 15px;
  margin-bottom: 40px;
}

.page-header h1 {
  font-size: 3rem;
  margin: 0;
  font-weight: bold;
}

.subtitle {
  font-size: 1.3rem;
  margin: 10px 0 0 0;
  opacity: 0.9;
}

.welcome-section {
  text-align: center;
  margin-bottom: 50px;
  padding: 30px;
  background: #f8f9fa;
  border-radius: 10px;
}

.welcome-section h2 {
  color: #333;
  font-size: 2.2rem;
  margin-bottom: 15px;
}

.current-date {
  font-style: italic;
  color: #666;
  margin-top: 15px;
}

.destinations-section {
  margin-bottom: 50px;
}

.destinations-section h3 {
  font-size: 2rem;
  text-align: center;
  margin-bottom: 30px;
  color: #333;
}

.destinations-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 30px;
  margin-bottom: 40px;
}

.destination-card {
  background: white;
  border-radius: 15px;
  box-shadow: 0 8px 25px rgba(0,0,0,0.1);
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.destination-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 15px 35px rgba(0,0,0,0.2);
}

.destination-card img {
  width: 100%;
  height: 220px;
  object-fit: cover;
}

.card-content {
  padding: 25px;
}

.card-content h4 {
  font-size: 1.5rem;
  margin-bottom: 10px;
  color: #333;
}

.card-content p {
  color: #666;
  line-height: 1.6;
  margin-bottom: 20px;
}

.price-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.price {
  font-size: 1.3rem;
  font-weight: bold;
  color: #4a90e2;
}

.duration {
  background: #e9f4ff;
  padding: 5px 15px;
  border-radius: 20px;
  font-size: 0.9rem;
  color: #4a90e2;
}

.stats-section {
  text-align: center;
  margin-bottom: 50px;
  padding: 50px 20px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  border-radius: 15px;
}

.stats-section h3 {
  font-size: 2rem;
  margin-bottom: 40px;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 30px;
}

.stat-item h4 {
  font-size: 3rem;
  margin: 0;
  font-weight: bold;
}

.stat-item p {
  font-size: 1.1rem;
  margin: 10px 0 0 0;
  opacity: 0.9;
}

.contact-section {
  background: #f8f9fa;
  padding: 50px 30px;
  border-radius: 15px;
}

.contact-section h3 {
  text-align: center;
  font-size: 2rem;
  margin-bottom: 30px;
  color: #333;
}

.contact-info {
  text-align: center;
  margin-bottom: 40px;
}

.contact-info p {
  margin: 10px 0;
  font-size: 1.1rem;
  color: #555;
}

.contact-form {
  max-width: 600px;
  margin: 0 auto;
}

.contact-form h4 {
  text-align: center;
  margin-bottom: 25px;
  color: #333;
}

.contact-form form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.contact-form input,
.contact-form textarea {
  padding: 15px;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 1rem;
  transition: border-color 0.3s ease;
}

.contact-form input:focus,
.contact-form textarea:focus {
  outline: none;
  border-color: #4a90e2;
}

.btn-primary {
  background: linear-gradient(135deg, #4a90e2, #357abd);
  color: white;
  padding: 15px 30px;
  border: none;
  border-radius: 8px;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-primary:hover {
  background: linear-gradient(135deg, #357abd, #2968a3);
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}

.btn-primary:disabled {
  opacity: 0.7;
  cursor: not-allowed;
  transform: none;
}

.message {
  margin-top: 20px;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
  font-weight: bold;
}

.message.success {
  background: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.message.error {
  background: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

/* Responsividade */
@media (max-width: 768px) {
  .page-header h1 {
    font-size: 2.2rem;
  }
  
  .subtitle {
    font-size: 1.1rem;
  }
  
  .destinations-grid {
    grid-template-columns: 1fr;
  }
  
  .stats-grid {
    grid-template-columns: 1fr;
  }
  
  .stat-item h4 {
    font-size: 2.5rem;
  }
}
</style>