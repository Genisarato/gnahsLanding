<script setup>
import { Head } from '@inertiajs/vue3';
import { computed, ref, onMounted, onUnmounted } from 'vue';
import { wTrans } from 'laravel-vue-i18n';

const selectedFilter = ref('all');

const isScrolled = ref(false);

const handleScroll = () => {
    isScrolled.value = window.scrollY > 50;
};

onMounted(() => {
    window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll);
});

const allCards = computed(() => [
    {
        id: 1,
        title: wTrans('cards.card1.title').value,
        description: wTrans('cards.card1.description').value,
        tags: [wTrans('filter.technology').value],
        image: '/images/image-1.jpg',
        imagePosition: 'left'
    },
    {
        id: 2,
        title: wTrans('cards.card2.title').value,
        description: wTrans('cards.card2.description').value,
        tags: [wTrans('filter.marketing').value, wTrans('filter.technology').value], 
        image: '/images/image-2.jpg', 
        imagePosition: 'right'
    }
]);

const availableTags = computed(() => {
    const tagsSet = new Set();
    allCards.value.forEach(card => {
        card.tags.forEach(tag => tagsSet.add(tag));
    });
    return Array.from(tagsSet);
});

const filteredCards = computed(() => {
    if (selectedFilter.value === 'all') {
        return allCards.value;
    }
    return allCards.value.filter(card => card.tags.includes(selectedFilter.value));
});
</script>

<template>
    <Head :title="$t('brand.name')" />
    
    <div class="min-h-screen bg-white font-sans text-gray-800 flex flex-col justify-between">
        
        <header class="relative w-full">
            
            <div 
                class="w-full flex flex-col sm:flex-row justify-between items-center sm:items-start p-6 md:px-12 md:py-4 gap-4 fixed top-0 left-0 right-0 z-50 transition-all duration-300"
                :class="isScrolled ? 'bg-[#E00069] shadow-md' : 'bg-transparent'"
            >
                <img 
                    :src="isScrolled ? '/images/logo-gnahs-full-white.png' : '/images/logo-gnahs.png'" 
                    :alt="$t('brand.name')" 
                    class="h-12 md:h-14 object-contain transition-all duration-300"
                >
                <nav class="flex flex-wrap justify-center items-center gap-4 md:gap-8 text-white font-medium text-sm md:text-base h-full sm:pt-2">
                    <a href="#" class="hover:text-gray-200 transition-colors">{{ $t('nav.hotel') }}</a>
                    <a href="#" class="hover:text-gray-200 transition-colors">{{ $t('nav.offers') }}</a>
                    <a href="#" class="border border-white rounded-full px-5 py-1.5 md:px-6 md:py-1.5 hover:bg-white hover:text-gray-900 transition-colors whitespace-nowrap">
                        {{ $t('nav.book') }}
                    </a>
                </nav>
            </div>
            
            <div class="relative min-h-[500px] md:h-[818px] bg-[linear-gradient(to_bottom,#1E1E1F40_0%,#1E1E1F00_40%),url('/images/girona.jpg')] bg-cover bg-center bg-no-repeat flex items-center justify-center w-full">
                <div class="w-full px-6 pointer-events-none pt-24">
                    <h1 class="text-white text-4xl sm:text-5xl md:text-7xl lg:text-[96px] font-bold uppercase tracking-wider text-center drop-shadow-md leading-none max-w-5xl mx-auto">
                        {{ $t('brand.name') }}
                    </h1>
                </div>
            </div>

        </header>

        <section class="mx-auto px-6 md:px-[30px] pt-12 md:pt-16 pb-12 md:pb-[66px] w-full max-w-[1180px]">
            <h2 class="text-center text-3xl sm:text-4xl md:text-[48px] leading-tight text-black mb-6 md:mb-10">
                {{ $t('brand.name') }}:<br>{{ $t('intro.subtitle') }}
            </h2>
            
            <div class="text-gray-800 text-sm md:text-base leading-relaxed text-justify md:text-left space-y-6">
                <p>{{ $t('intro.p1') }}</p>
                <p>{{ $t('intro.p2') }}</p>
            </div>
        </section>

        <section class="w-full bg-[#F1D0D6] px-6 md:px-[30px] py-4 md:py-0 md:h-[73px] flex items-center sticky top-[116px] md:top-[88px] z-40 shadow-md">
            <div class="mx-auto max-w-[1180px] w-full flex flex-col sm:flex-row items-center gap-4 sm:gap-0">
                <span class="sm:mr-6 text-xl md:text-[24px] text-gray-800 font-medium sm:font-normal">{{ $t('filter.label') }}</span>
                
                <select 
                    v-model="selectedFilter"
                    class="w-full sm:w-[324px] h-[48px] md:h-[53px] rounded-full border-transparent shadow-sm px-6 md:px-8 text-base md:text-lg text-gray-700 focus:border-transparent focus:ring-2 focus:ring-pink-400 cursor-pointer"
                >
                    <option value="all">{{ $t('filter.select') }}</option>
                    
                    <option v-for="tag in availableTags" :key="tag" :value="tag">
                        {{ tag }}
                    </option>
                </select>
            </div>
        </section>
                
        <main class="mx-auto max-w-[1180px] w-full px-6 md:px-[30px] pt-12 md:pt-[75px] pb-16 md:pb-[80px] flex flex-col gap-12 md:gap-[75px]">
            <article 
                v-for="card in filteredCards" 
                :key="card.id" 
                class="flex flex-col gap-6 md:gap-[50px] md:flex-row items-center md:items-start"
                :class="{'md:flex-row-reverse': card.imagePosition === 'right'}"
            >
                <div class="w-full md:w-[400px] h-[240px] sm:h-[300px] flex-shrink-0">
                    <img 
                        :src="card.image" 
                        :alt="card.title" 
                        class="w-full h-full object-cover rounded-[20px] md:rounded-[25px]"
                    >
                </div>

                <div class="flex flex-col gap-[15px] flex-grow w-full">
                    <div class="flex flex-wrap gap-2 md:gap-[15px]">
                        <span 
                            v-for="tag in card.tags" 
                            :key="tag"
                            class="bg-[#E00069] text-white text-xs md:text-sm px-4 py-1.5 rounded-full"
                        >
                            {{ tag }}
                        </span>
                    </div>

                    <h3 class="text-2xl md:text-[26px] font-bold text-gray-900 text-center md:text-left">{{ card.title }}</h3>

                    <p class="text-gray-800 text-sm md:text-base leading-relaxed text-justify">
                        {{ card.description }}
                    </p>

                    <div class="text-center md:text-left">
                        <a href="#" class="text-[#e4006c] font-medium underline mt-2 text-sm md:text-base inline-block hover:text-pink-600">
                            {{ $t('cards.more_info') }}
                        </a>
                    </div>
                </div>
            </article>
        </main>

        <footer class="bg-[#F1D0D6] w-full pt-10 pb-6 px-6 md:px-[60px] flex flex-col justify-between gap-8">
            <div class="mx-auto max-w-[1180px] w-full flex flex-col md:flex-row justify-between items-center md:items-start gap-8">
                <div>
                    <img src="/images/logo-gnahs.png" :alt="$t('brand.name')" class="h-12 md:h-16 object-contain">
                </div>
                <div class="flex flex-col sm:flex-row text-center sm:text-left gap-8 md:gap-[80px] text-gray-800 text-sm md:text-base">
                    <div class="flex flex-col gap-3 md:gap-[20px]">
                        <a href="#" class="hover:underline">{{ $t('footer.contact') }}</a>
                        <a href="#" class="hover:underline">{{ $t('footer.newsletter') }}</a>
                        <a href="#" class="hover:underline">{{ $t('footer.work_with_us') }}</a>
                    </div>
                    <div class="flex flex-col gap-3 md:gap-[20px]">
                        <a href="#" class="hover:underline">{{ $t('footer.cookies') }}</a>
                        <a href="#" class="hover:underline">{{ $t('footer.privacy') }}</a>
                        <a href="#" class="hover:underline">{{ $t('footer.legal') }}</a>
                    </div>
                </div>
            </div>
            <div class="mx-auto max-w-[1180px] w-full text-center md:text-right text-sm md:text-base text-gray-800 border-t border-pink-200/40 pt-4">
                {{ $t('footer.developed_by') }} <span class="text-[#e4006c] font-medium">{{ $t('brand.name') }}</span>
            </div>
        </footer>

    </div>
</template>