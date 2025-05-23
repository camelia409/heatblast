<template>
  <main class="flex-grow">
    <!-- Hero Section (Unchanged) -->
    <section 
      class="bg-cover md:bg-center relative -z-10 w-full h-60 sm:h-80 md:h-94 animate-fadeIn"
      :style="{ backgroundImage: `url(${backgroundImage})` }"
    >
      <div class="absolute top-0 left-0 w-full h-full bg-black opacity-50"></div>
      <div class="container py-8 md:py-24 text-center relative z-10 px-4">
        <h1 class="text-2xl sm:text-3xl md:text-4xl font-extrabold mb-4 drop-shadow-lg font-serif text-white animate-fadeInUp">
          Library
        </h1>
        <p class="text-sm sm:text-base md:text-lg max-w-full sm:max-w-3xl mx-auto mb-4 md:mb-8 leading-relaxed drop-shadow-lg font-sans text-white animate-fadeInUp">
          Explore our extensive library resources and services, including books, e-journals, and more.
        </p>
      </div>
    </section>

    <!-- Vertical Tabs Section with Dynamic Content Section Background -->
    <section class="mx-auto py-6 md:py-12 px-4 md:px-10 bg-cover bg-center" :style="{ backgroundImage: `url(${aurccBg})` }">
      <div class="relative flex flex-col px-8 md:flex-row">
        <!-- Mobile Section Dropdown -->
        <div class="md:hidden flex-shrink-0 w-64 bg-gradient-to-r from-[#21209c] to-blue-600 rounded-lg shadow-lg p-4 mb-4 md:mb-0 md:ml-0 ml-3 top-4 h-max">
          <div class="relative p-4 rounded-lg text-center">
            <h2 class="text-2xl font-serif text-white font-semibold animate-slideIn">Sections</h2>
          </div>
          <div class="space-y-2 font-serif">
            <button
              v-for="(section, index) in sections"
              :key="index"
              @click="scrollToSection(section)"
              :class="{'bg-yellow-400 text-[#23120b]': activeSection === section, 'bg-gray-100': activeSection !== section}"
              class="w-full py-2 px-4 rounded-md font-semibold hover:bg-yellow-400 hover:text-[#23120b] transition-colors duration-200 animate-slideIn"
            >
              {{ section }}
            </button>
          </div>
        </div>

        <!-- Desktop Vertical Tabs -->
        <div class="hidden md:block flex-shrink-0 w-64 bg-gradient-to-r from-[#21209c] to-blue-600 rounded-lg shadow-lg p-4 mb-4 md:mb-0 md:mr-4 sticky top-4 h-max">
          <div class="relative p-4 rounded-lg text-center">
            <h2 class="text-2xl font-serif text-white font-semibold animate-slideIn">Sections</h2>
          </div>
          <div class="space-y-2 font-serif">
            <button
              v-for="(section, index) in sections"
              :key="index"
              @click="scrollToSection(section)"
              :class="{'bg-yellow-400 text-[#23120b]': activeSection === section, 'bg-gray-100': activeSection !== section}"
              class="w-full py-2 px-4 rounded-md font-semibold hover:bg-yellow-400 hover:text-[#23120b] transition-colors duration-200 animate-slideIn"
            >
              {{ section }}
            </button>
          </div>
        </div>

        <!-- Tab Content -->
        <div class="w-full px-0 md:px-10 font-serif min-h-[400px] max-h-[600px] md:max-h-[800px] overflow-y-auto">
          <!-- About Library Section -->
          <div v-if="activeSection === 'About Library'" ref="AboutLibrary" class="bg-white rounded-lg shadow-lg p-4 md:p-6 mb-16 animate-fadeIn">
            <h3 class="text-2xl md:text-3xl font-bold text-black mb-4">About Library</h3>
            <p class="text-base text-lg md:text-xl rounded-b-lg">{{ libraryData.description }}</p>
          </div>

          <!-- E-journals Section -->
          <div v-if="activeSection === 'E-journals'" ref="EJournals" class="bg-white rounded-lg shadow-lg p-4 md:p-6 mb-16 animate-fadeIn">
            <h3 class="text-2xl md:text-3xl font-bold text-black mb-4">E-journals</h3>
            <p class="text-base text-lg md:text-xl mb-4 rounded-b-lg">{{ libraryData.e_journals.description }}</p>
            <ul class="list-disc pl-5 text-sm md:text-xl text-gray-900 mb-4">
              <li v-for="feature in libraryData.e_journals.library_salient_features" :key="feature" class="mb-2">
                {{ feature }}
              </li>
            </ul>
            <p class="text-lg md:text-xl font-semibold mb-2">Library Services:</p>
            <ul class="list-disc pl-5 text-sm md:text-xl text-gray-900">
              <li v-for="service in libraryData.e_journals.library_services" :key="service" class="mb-2">
                {{ service }}
              </li>
            </ul>
          </div>

          <!-- Other sections... -->
          <div v-if="activeSection === 'Library Resources'" ref="LibraryResources" class="bg-white rounded-lg shadow-lg p-4 md:p-6 mb-16 animate-fadeIn">
            <h3 class="text-2xl md:text-3xl font-bold text-black mb-4">Library Resources</h3>
            <p class="text-base md:text-xl mb-4 rounded-b-lg">{{ libraryData.library_resources.description }}</p>
            
            <!-- Mobile View: Cards -->
            <div class="md:hidden space-y-4">
              <div v-for="(value, key) in libraryData.library_resources.collection" :key="key" 
                class="bg-gray-50 p-4 rounded-lg">
                <h4 class="font-semibold text-gray-700 mb-2">{{ formatTitle(key) }}</h4>
                <p class="text-gray-900">{{ value }}</p>
              </div>
              <div class="bg-gray-50 p-4 rounded-lg">
                <h4 class="font-semibold text-gray-700 mb-2">Journal Binding Volumes</h4>
                <p class="text-gray-900">{{ libraryData.library_resources.journal_binding.volumes }}</p>
              </div>
            </div>

            <!-- Desktop View: Table -->
            <div class="hidden md:block overflow-x-auto">
              <table class="min-w-full bg-white border border-gray-200">
                <thead>
                  <tr>
                    <th class="px-4 py-2 border-b text-left">Books</th>
                    <th class="px-4 py-2 border-b text-left">Reference Books</th>
                    <th class="px-4 py-2 border-b text-left">CDs</th>
                    <th class="px-4 py-2 border-b text-left">Donation Books</th>
                    <th class="px-4 py-2 border-b text-left">Bound Volumes</th>
                    <th class="px-4 py-2 border-b text-left">Project Reports</th>
                    <th class="px-4 py-2 border-b text-left">Subscription to Journals</th>
                    <th class="px-4 py-2 border-b text-left">Journal Binding Volumes</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td class="px-4 py-2 border-b">{{ libraryData.library_resources.collection.books }}</td>
                    <td class="px-4 py-2 border-b">{{ libraryData.library_resources.collection.reference_books }}</td>
                    <td class="px-4 py-2 border-b">{{ libraryData.library_resources.collection.cds }}</td>
                    <td class="px-4 py-2 border-b">{{ libraryData.library_resources.collection.donation_books_collection }}</td>
                    <td class="px-4 py-2 border-b">{{ libraryData.library_resources.collection.bound_volumes_of_journals }}</td>
                    <td class="px-4 py-2 border-b">{{ libraryData.library_resources.collection.project_reports }}</td>
                    <td class="px-4 py-2 border-b">{{ libraryData.library_resources.collection.subscription_to_journals }}</td>
                    <td class="px-4 py-2 border-b">{{ libraryData.library_resources.journal_binding.volumes }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

        </div>
      </div>
    </section>
  </main>
</template>


<script>
import backgroundImage from '@/assets/library.webp'; // Hero section background image
import aurccBg from '@/assets/aurcc_bg.webp'; // Content section background image
import libraryData from '../assets/library.json';

export default {
  data() {
    return {
      backgroundImage,
      aurccBg, // Use this variable for the content section background
      libraryData,
      sections: [
        'About Library',
        'E-journals',
        'Library Resources',
        'Digital Library',
        'Open Access Resources',
        'Library Sections',
        'Faculty'
      ],
      activeSection: 'About Library',
    };
  },
  mounted() {
    this.fetchLibraryData();
  },
  methods: {
    async fetchLibraryData() {
      try {
        // Simulate fetching data
        await new Promise(resolve => setTimeout(resolve, 1000));
      } catch (err) {
        console.error('Failed to load library data:', err);
      }
    },
    scrollToSection(section) {
      const element = this.$refs[section.replace(/ /g, '')];
      if (element) {
        element.scrollIntoView({ behavior: 'smooth' });
      }
      this.activeSection = section;
    },
    formatTitle(title) {
      return title.replace(/([A-Z])/g, ' $1').trim();
    }
  }
};
</script>


<style scoped>
/* Adjustments to customize layout and appearance */
.container {
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
}

@media (min-width: 768px) {
  /* Ensure responsive and consistent styling for larger devices */
  .animate-slideIn {
    animation: slideIn 0.5s ease-in-out;
  }

  @keyframes slideIn {
    from {
      opacity: 0;
      transform: translateX(-100%);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }
}
</style>
