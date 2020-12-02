<template>
         <v-data-table
         v-model="selected"
    :headers="headers"
    :items="catalogueItems"
    class="elevation-1"
    style="width:100%"
    show-select
  >
    <template v-slot:top>
        <v-dialog
      v-model="detailedInfoDialog"
      persistent
      max-width="600px"
    >
      <v-card>
        <v-card-text>
          <v-container>
             <v-card-title class="pl-0">
            <v-list-item-avatar class="ml-0" size="60">
                          <iframe :src="editedItem.image" width="100%" height="200px"></iframe>
                    </v-list-item-avatar>
                    <div>
                      <v-row
      align="center"
      justify="center"
      class="mb-2 ml-1"
    >
      <v-badge
        bordered
        color="success"
        icon="mdi-check"
        overlap
      >
        <v-btn
          class="white--text"
          color="primary"
          depressed
          small
          style="font-size:10px"
        >
          Fit for sale
        </v-btn>
      </v-badge>
       <div class="mx-1"></div>
      <v-badge
        bordered
        color="success"
        icon="mdi-check"
        overlap
      >
        <v-btn
          class="white--text"
          color="primary"
          depressed
          small
           style="font-size:10px"
        >
         Repeat Order
        </v-btn>
      </v-badge>
       <div class="mx-1"></div>
      <v-badge
        bordered
        color="success"
        icon="mdi-check"
        overlap
      >
        <v-btn
          class="white--text"
          color="primary"
          depressed
          small
           style="font-size:10px"
        >
          Export Compliant
        </v-btn>
      </v-badge>

      <div class="mx-0">
        </div>
        </v-row>
          <span class="font-weight-bold headline">{{editedItem.title}}</span>
                    </div>
        </v-card-title>
            <v-row>
               <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Category"
                  :value="editedItem.category"
                  required></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Sub Category"
                  :value="editedItem.subCategory"
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Style Code"
                  :value="editedItem.styleCode"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
               <v-col
                cols="12"
                sm="6"
                md="6"
              >
                <v-text-field
                  label="Stock (USD)"
                  :value="editedItem.price1"
                  required></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="6"
              >
                <v-text-field
                  label="Repeat Order (USD)"
                  :value="editedItem.price2"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
             <v-row>
               <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Quantity"
                  :value="editedItem.quantity"
                  required></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Finish"
                  :value="editedItem.finish"
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Ex-Factory (Max Days)"
                  :value="editedItem.maxExFactory"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  label="Image"
                  :value="editedItem.image"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
            <v-expansion-panels>
    <v-expansion-panel
      v-for="(item,i) in editedItem.expansion"
      :key="i"
    >
      <v-expansion-panel-header>
        {{item.title}}
      </v-expansion-panel-header>
      <v-expansion-panel-content v-for="itemDetail in item.children" :key="itemDetail">
        <span class="font-weight-bold">{{itemDetail.title}}</span>: {{itemDetail.value}}
      </v-expansion-panel-content>
    </v-expansion-panel>
  </v-expansion-panels>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="detailedInfoDialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="detailedInfoDialog = false"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-btn
        small
        class="mr-2"
        @click="editItem(item)"
      >
        View
      </v-btn>
    </template>
  </v-data-table>
</template>

<script>
export default {
  name: 'Dashboard',
  props: {
    headers: Array,
    catalogueItems: Array
  },
  data () {
    return {
      selected: [],
      detailedInfoDialog: false,
      editedIndex: -1,
      editedItem: {
        id: 0,
        title: '',
        categoryId: 0,
        subCategoryId: 0,
        styleCode: '',
        price1: '',
        price2: '',
        quantity: '',
        finish: '',
        size: '',
        maxExFactory: '',
        lables: '',
        fitForSale: '',
        repeatOrders: '',
        additionalInformation: '',
        innerDimensions: '',
        piecePackingWeight: '',
        ROLeadTime: '',
        RoMOQ: '',
        paymentTerms: '',
        exportCompliance: '',
        image: '',
        status: '',
        expansion: []
      },
      defaultItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0
      }
    }
  },
  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    }
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    }
  },

  methods: {
    editItem (item) {
      this.editedIndex = this.catalogueItems.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.detailedInfoDialog = true
    },

    deleteItem (item) {
      this.editedIndex = this.catalogueItems.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm () {
      this.desserts.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close () {
      this.detailedInfoDialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    }

  }
}
</script>
