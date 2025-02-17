<template>
    <v-form v-model="valid" ref="form" lazy-validation>
      <v-container>
        <!-- Tarih Alanı -->
        <v-menu
          ref="menu"
          v-model="menu"
          :close-on-content-click="false"
          transition="scale-transition"
          offset-y
          min-width="auto"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="form.tarih"
              label="Tarih*"
              prepend-icon="mdi-calendar"
              readonly
              v-bind="attrs"
              v-on="on"
              :rules="[v => !!v || 'Tarih zorunlu', dateRules]"
            ></v-text-field>
          </template>
          <v-date-picker
            v-model="form.tarih"
            :max="maxDate"
            :min="minDate"
            locale="tr"
            no-title
            scrollable
            @input="menu = false"
          ></v-date-picker>
        </v-menu>
  
        <!-- Birim Seçimi -->
        <v-select
          v-model="form.birim"
          :items="birimler"
          label="Birim*"
          :rules="[v => !!v || 'Birim seçimi zorunlu']"
          required
        ></v-select>
  
        <!-- İl Seçimi -->
        <v-select
          v-model="form.il"
          :items="iller"
          label="İl*"
          :rules="[v => !!v || 'İl seçimi zorunlu']"
          required
        ></v-select>
  
        <!-- Departman -->
        <v-select
          v-model="form.departman"
          :items="departmanlar"
          label="Departman*"
          :rules="[v => !!v || 'Departman seçimi zorunlu']"
          required
        ></v-select>
  
        <!-- Kategori -->
        <v-select
          v-model="form.kategori"
          :items="kategoriler"
          label="Kategori*"
          :rules="[v => !!v || 'Kategori seçimi zorunlu']"
          required
        ></v-select>
  
        <!-- Açıklama -->
        <v-textarea
          v-model="form.aciklama"
          label="Açıklama*"
          :rules="[v => !!v || 'Açıklama zorunlu']"
          required
          rows="3"
        ></v-textarea>
  
        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="kaydet"
        >
          Kaydet
        </v-btn>
      </v-container>
    </v-form>
  </template>
  
  <script>
  export default {
    data: () => ({
      valid: false,
      menu: false,
      form: {
        tarih: null,
        birim: null,
        il: null,
        departman: null,
        kategori: null,
        aciklama: null,
      },
      // Örnek veriler - Kendi verilerinizle değiştirin
      birimler: ['Birim 1', 'Birim 2', 'Birim 3'],
      iller: ['İstanbul', 'Ankara', 'İzmir'],
      departmanlar: ['Muhasebe', 'IT', 'İnsan Kaynakları'],
      kategoriler: ['Kategori 1', 'Kategori 2', 'Kategori 3'],
    }),
  
    computed: {
      minDate() {
        const date = new Date();
        date.setMonth(date.getMonth() - 1);
        return date.toISOString().substr(0, 10);
      },
      maxDate() {
        return new Date().toISOString().substr(0, 10);
      },
      dateRules() {
        return (date) => {
          const selectedDate = new Date(date);
          const minDate = new Date(this.minDate);
          const maxDate = new Date(this.maxDate);
          return (selectedDate >= minDate && selectedDate <= maxDate) || 'Geçersiz tarih aralığı';
        };
      },
    },
  
    methods: {
      kaydet() {
        if (this.$refs.form.validate()) {
          // Form verilerini işleme al
          console.log(this.form);
          alert('Form başarıyla gönderildi!');
          this.$refs.form.reset();
        }
      },
    },
  };
  </script>