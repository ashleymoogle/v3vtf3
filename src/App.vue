<script setup lang="ts">
    import { ref, computed, watch } from 'vue';

    const percentage = ((value: any, decimals = 0): any => {
        return value.toLocaleString(`en-CA`, {
            maximumFractionDigits: decimals,
            minimumFractionDigits: decimals,
            style: 'percent',
        });
    });

    const catSelected = ref('');
    const wrapperKey = ref(0);

    const examCanvas = ref<any>({});
    const examDocument = ref<any>({});

    const zoom = ref('100%');
    const zoomRatio = computed(() => (parseInt(zoom.value) || 1) / 100);

    const setZoom = (zoomLevel: any) => {
        console.log('pass');
        if (zoomLevel === 'Fit') {

            const availableWidth = examCanvas.value.clientWidth - 2 * 16;

            zoomLevel = availableWidth / examDocument.value.$el.clientWidth;
        } else {
            zoomLevel = (parseInt(zoomLevel) / 100) || 1;
        }

        zoom.value = percentage(zoomLevel) as string;
        // document.body.style.zoom = zoomLevel.toString()
        wrapperKey.value++;
    };

    const menuProps = computed(() => {
        return zoomRatio.value >= 1 ? {} : {};
    })

    watch(zoom, () => {
        wrapperKey.value++;
    })
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
    <div id="wrapperZoom">
        Ratio: {{zoomRatio}}
        <div class=" class=mt-4exam__zoom">
            <v-combobox
                :value="zoom"
                hide-details
                outlined
                maxlength="4"
                :menu-props="menuProps"
                label="Zoom level"
                style="z-index: 20"
                :items="['Fit', '50%', '75%', '100%', '125%', '150%', '200%']"
                @update:modelValue="setZoom"
            />
        </div>
    </div>
    <div class="wrapper" id="wrapper" :key="wrapperKey" :style="`zoom:${zoomRatio}`">
        <div ref="examCanvas" class="exam__canvas">
            <div ref="examDocument" :style="`width:100%;height:100%;`">
                <div>
                <!-- <div :style="`zoom: ${zoomRatio}`"> -->
                    <v-combobox
                        v-model="catSelected"
                        label="Type of cat"
                        hide-details
                        :menu-props="{
                            contained: true
                        }"
                        outlined
                        maxlength="4"
                        style="z-index: 20"
                        :items="['Black cat', 'White cat', 'Tabby cat']"
                    />
                    <div class="ma-4 text-center">
                        <v-menu v-bind="menuProps" :contained="true">
                            <template v-slot:activator="{ props }">
                                <v-btn
                                    color="primary"
                                    v-bind="props"
                                >
                                    Dropdown
                                </v-btn>
                            </template>

                            <v-list>
                                <v-list-item
                                    v-for="(item, index) in [
                                        { title: 'Click Me' },
                                        { title: 'Click Me 2' },
                                        { title: 'Click Me 3' },
                                        { title: 'Click Me 4' },
                                      ]"
                                    :key="index"
                                >
                                    <v-list-item-title>{{ item.title }}</v-list-item-title>
                                </v-list-item>
                            </v-list>
                        </v-menu>
                    </div>
                    <p v-if="catSelected" class="ma-4">
                        My selected cat is {{ catSelected }}.
                    </p>
                    <v-divider class="ma-4" />
                    <p>Bacon ipsum dolor amet t-bone tenderloin sirloin ball tip ham chislic short loin jerky chicken. Prosciutto swine tongue, shank alcatra t-bone salami andouille ham hock picanha meatball biltong cupim spare ribs kevin. Kevin pig turkey shank, alcatra jerky short loin chislic. Pork loin bacon chuck pancetta beef cow ribeye rump pork chop turducken bresaola.

                        Chuck shank tri-tip chislic ham hock chicken sirloin fatback meatloaf flank pancetta venison tail ribeye tenderloin. Landjaeger shankle bresaola ribeye, boudin filet mignon alcatra chuck. Sirloin hamburger pancetta chislic, flank ground round frankfurter. Beef short ribs short loin capicola turducken salami landjaeger pork chop pig beef ribs shank ham prosciutto ball tip flank.

                        Fatback tail swine tongue, chislic filet mignon pork chop meatball capicola pork loin kevin beef ribs venison ham hock. Picanha shoulder doner jowl turducken. Pork loin rump hamburger, chicken andouille pig shankle ribeye drumstick pancetta meatball. Ball tip kielbasa burgdoggen pastrami shankle kevin beef picanha pork belly.

                        Pork loin drumstick chuck hamburger tenderloin turkey. Shankle short loin andouille ham venison, ribeye doner porchetta pastrami shank. Corned beef pork loin drumstick sausage shank kielbasa. Salami shankle tri-tip shoulder doner beef ribs beef short loin. Chislic shoulder brisket, filet mignon sirloin beef jerky. Short loin pancetta swine ribeye brisket salami pork chop leberkas tenderloin cupim frankfurter landjaeger. Pastrami pork chop sausage strip steak salami.

                        T-bone biltong ham ground round leberkas. Hamburger bresaola salami, burgdoggen ball tip pork belly strip steak. Capicola chislic tongue, ground round pork belly t-bone chicken porchetta filet mignon. Fatback ham tail, kevin tongue shank rump pork loin sirloin short loin beef capicola.</p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
.exam__canvas{flex-grow:1;height:100%;overflow:auto;padding:16px}
</style>
