<template>
  
    <div class = "kiiras">
      <h1 v-if = "loading">Dokumentum</h1>
      <h1 v-else-if = "data">Dokumentum ID  - {{ docID }}</h1>

      <div v-if="loading">Betöltés...</div>

      <div v-else-if = "data">
        <p>Feszültség: {{ data.feszultseg }}</p>
        <p>Százalék: {{ data.szazalek }}</p>
        <p>Merítés Kezdes: {{ datum(data.merites_kezdes) }}</p>
        <p>Merítés Vége: {{ datum(data.merites_vege) }}</p>
        <p>Töltés Kezdes: {{ datum(data.toltes_kezdes) }}</p>
        <p>Töltés Vége: {{ datum(data.toltes_vege) }}</p>
        <p>Jó Rossz: {{ data.jo_rossz }}</p>
      </div>

      <div v-else>
        <p>Nem található ilyen ID-vel rendelkező dokumentum.</p>
      </div>

    </div>
  
</template>

<script>
import { databases } from '@/appwrite';              //
import ids from '@/appwrite/ids.json';               // ID-k importálása
import moment from 'moment/min/moment-with-locales'; // Dátum

export default {
  data() {
    return {
      data: null,
      loading: true,
      docID:'67abb6110001665eb916'
    }
  },
  mounted()
  {
    this.lekeres();
  },
  
  methods:{
    async lekeres() {
      try {
        const response = await databases.getDocument(ids.database_id, ids.akkumulator_id, this.docID);
        this.data = response;
      } catch (error) {
        console.error("Error fetching data:", error);
      } finally {
        this.loading = false;
      }
    },
    
    datum(a)
    {
      moment.locale('hu');
      return moment(a).format("LL");
    }
  }
};
</script>

<style scoped>
</style>
