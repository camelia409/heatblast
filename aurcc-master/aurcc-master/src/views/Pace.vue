<template>
  <main class="flex-grow">
    <!-- Hero Section -->
    <section 
      class="bg-cover bg-center relative w-full h-40 sm:h-80 md:h-94 animate-fadeIn" 
      :style="{ backgroundImage: `url(${backgroundImage})` }">
      
      <!-- Gradient Overlay -->
      <div class="absolute inset-0 bg-black opacity-20"></div>

      <!-- Hero Content -->
      <div class="flex items-center justify-center h-full relative z-10 px-4">
        <h1 class="text-xl md:text-4xl font-extrabold text-white drop-shadow-lg font-serif text-center animate-slideIn">
          PACE CELL
        </h1>
      </div>
    </section>

    <!-- Main Content Section -->
    <section 
      class="mx-auto py-6 md:py-12 md:px-10 bg-indigo-100 rounded-lg animate-popIn"
      :style="{ backgroundImage: `url(${aurccBackgroundImage})`, backgroundSize: 'cover', backgroundPosition: 'center' }">

      <div class="relative flex flex-col md:flex-row px-4 md:px-8">
        
        <!-- Vertical Tabs -->
        <div class="px-0 md:px-0">
          <div class="flex-shrink-0 w-full md:w-64 bg-gradient-to-r from-[#21209c] to-blue-600 rounded-lg shadow-lg p-4 mb-4 md:mb-0 md:mr-4">
            
            <div class="relative p-4 rounded-lg">
              <h2 class="text-2xl text-white font-serif text-center font-semibold">Sections</h2>
            </div>

            <div class="space-y-2 font-serif">
              <button 
                @click="currentSection = 'coordinators'" 
                :class="tabButtonClass('coordinators')"
              >Coordinators</button>

              <button 
                v-if="paceData.support_staff.length" 
                @click="currentSection = 'support_staff'" 
                :class="tabButtonClass('support_staff')"
              >Support Staff</button>

              <button 
                v-if="paceData.student_volunteers.length" 
                @click="currentSection = 'student_volunteers'" 
                :class="tabButtonClass('student_volunteers')"
              >Student Volunteers</button>
            </div>
          </div>
        </div>

        <!-- Tab Content -->
        <div class="w-full px-0 md:px-10 font-serif min-h-[400px] max-h-[600px] md:max-h-[800px] overflow-y-auto">
          <!-- Coordinators -->
          <div 
            v-if="currentSection === 'coordinators'" 
            class="bg-white rounded-lg shadow-lg p-4 md:p-6 mb-16 animate-fadeIn">
            <h2 class="text-2xl md:text-3xl font-bold text-black mb-4">Coordinators</h2>
            <div class="space-y-4">
              <div 
                v-for="(coordinator, index) in paceData.coordinators" 
                :key="index" 
                class="bg-white rounded-lg shadow-lg p-6 border border-gray-100">
                <p class="text-xl font-semibold"><strong>Name:</strong> {{ coordinator.name }}</p>
                <p class="text-xl"><strong>Position:</strong> {{ coordinator.position }}</p>
              </div>
            </div>
          </div>

          <!-- Support Staff -->
          <div 
            v-if="currentSection === 'support_staff'" 
            class="bg-white rounded-lg shadow-lg p-4 md:p-6 mb-16 animate-fadeIn">
            <h2 class="text-2xl md:text-3xl font-bold text-black mb-4">Support Staff</h2>
            <ul class="list-disc pl-5 text-base md:text-xl text-gray-900 space-y-2">
              <li v-for="(staff, index) in paceData.support_staff" :key="index">{{ staff }}</li>
            </ul>
          </div>

          <!-- Student Volunteers -->
          <div 
            v-if="currentSection === 'student_volunteers'" 
            class="bg-white rounded-lg shadow-lg p-4 md:p-6 mb-16 animate-fadeIn">
            <h2 class="text-2xl md:text-3xl font-bold text-black mb-4">Student Volunteers</h2>
            <ul class="list-disc pl-5 text-base md:text-xl text-gray-900 space-y-2">
              <li v-for="(volunteer, index) in paceData.student_volunteers" :key="index">{{ volunteer }}</li>
            </ul>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import paceData from '../assets/pace.json';
import backgroundImage from '@/assets/offices.webp';
import aurccBackgroundImage from '@/assets/aurcc_bg.webp';

export default {
  data() {
    return {
      paceData,
      backgroundImage,
      aurccBackgroundImage,
      currentSection: 'coordinators',
      showFull: false,
    };
  },
  methods: {
    tabButtonClass(section) {
      return [
        'w-full py-2 px-4 rounded-md font-semibold transition duration-300 ease-in-out',
        this.currentSection === section
          ? 'bg-yellow-400 text-[#23120b]'
          : 'bg-gray-100 hover:bg-yellow-400 hover:text-[#23120b]'
      ].join(' ');
    },
    showFullDescription() {
      this.showFull = true;
    }
  },
  computed: {
    truncatedDescription() {
      if (this.showFull || !this.isDescriptionLong) {
        return this.paceData.description;
      }
      const firstFullStop = this.paceData.description.indexOf('.');
      return this.paceData.description.slice(0, firstFullStop + 1);
    },
    isDescriptionLong() {
      return this.paceData.description.length > 100;
    }
  }
};
</script>

<style scoped>
.font-serif {
  font-family: 'Georgia', 'Times New Roman', Times, serif;
}

@keyframes fadeIn {
  0% { opacity: 0; transform: translateY(10px); }
  100% { opacity: 1; transform: translateY(0); }
}

@keyframes slideIn {
  0% { opacity: 0; transform: translateY(-20px); }
  100% { opacity: 1; transform: translateY(0); }
}

@keyframes popIn {
  0% { opacity: 0; transform: scale(0.95); }
  100% { opacity: 1; transform: scale(1); }
}

.animate-fadeIn {
  animation: fadeIn 0.8s ease-in-out;
}

.animate-slideIn {
  animation: slideIn 1s ease-in-out;
}

.animate-popIn {
  animation: popIn 0.9s ease-out;
}
</style>
