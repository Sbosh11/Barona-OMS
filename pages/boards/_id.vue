<template>
  <div
    class="d-flex flex-column board"
    :style="
      board.image.downloadURL != ''
        ? `background:url(${board.image.downloadURL});background-size:cover;`
        : board.color
        ? `background-color:${board.color}`
        : ''
    "
  >
    <div class="d-block">
      <v-container fluid class="jello-topbar">
        <div class="d-flex justify-space-between">
          <v-icon @click="drawer = true">mdi-menu</v-icon>
          <nuxt-link to="/">
            <v-row no-gutters align="center" justify="space-between">
              <h3 class="logo">Barona Fittings OMS</h3>
            </v-row>
          </nuxt-link>
          <v-icon small @click="deleteBoard()">mdi-delete-outline</v-icon>
        </div>
      </v-container>
      <v-navigation-drawer
        v-model="drawer"
        fixed
        left
        class="d-block px-3 py-3"
      >
        <v-container fluid class="jello-topbar">
          <v-row no-gutters align="center" justify="space-between">
            <v-icon @click="drawer = false">mdi-close</v-icon>

            <v-row no-gutters align="center" justify="end">
              <p class="jello-user">
                Signed in as<br />
                {{ $nuxt.$fire.auth.currentUser.email }}
              </p>
              &nbsp;
              <v-icon>mdi-account-circle-outline</v-icon>
            </v-row>
          </v-row>
        </v-container>
        <v-container class="d-block menu-items">
          <div class="d-flex flex-column">
            <div class="d-flex">
              <br />
            </div>
            <div class="d-flex">
              <nuxt-link to="/">
                <v-icon>mdi-view-dashboard-variant-outline</v-icon
                >&nbsp;&nbsp;<b>My Boards</b>
              </nuxt-link>
            </div>
            <div class="d-flex">
              <nuxt-link to="/auth/signout">
                <v-icon>mdi-exit-to-app</v-icon>&nbsp;&nbsp;<b>Sign out</b>
              </nuxt-link>
            </div>
          </div>
        </v-container>
      </v-navigation-drawer>
    </div>
    <h1>{{ board.title }}</h1>
    <small>created {{ board.dateCreated | formatDate }}</small>
    <div class="d-flex flex-row pr-6 pt-3">
      <div
        v-for="list in board.lists"
        @drop="drop($event, list.id)"
        @dragover="allowDrop($event)"
        class="d-flex flex-column pt-3 mr-6 list"
        v-bind:key="list.id"
      >
        <div class="d-flex flex-row justify-space-between">
          <h4>{{ list.title }}</h4>
          <v-icon small @click="deleteList(list.id)">mdi-delete-outline</v-icon>
        </div>

        <!--display cards-->

        <v-card
          v-for="card in list.cards"
          :draggable="true"
          @dragover.prevent
          @dragstart="drag($event, card)"
          class="mt-5"
          v-bind:key="card.id"
        >
          <v-btn light icon class="mr-4" @click="editCard(card)">
            <v-icon>mdi-pencil</v-icon>
          </v-btn>

          <v-list-group :value="true">
            <template v-slot:activator>
              <v-list-item-title>{{ card.title }}</v-list-item-title>
              <v-list-item-action> </v-list-item-action>
            </template>

            <v-list shaped>
              <v-list-item-group v-model="model" multiple>
                <template>
                  <v-btn
                    depressed
                    @click="
                      dialogProduct = true
                      cardId = card.id
                    "
                    class="mt-auto"
                    >Add Sub Product</v-btn
                  >

                  <v-list-item
                    v-for="product in card.products"
                    v-bind:key="product.id"
                    active-class="deep-purple--text text--accent-4"
                  >
                    <template v-slot:default="{ active }">
                      <v-list-item-content>
                        <v-list-item-title>Test</v-list-item-title>
                      </v-list-item-content>

                      <v-list-item-action>
                        <v-checkbox
                          :input-value="active"
                          color="deep-purple accent-4"
                        ></v-checkbox>
                      </v-list-item-action>
                    </template>
                  </v-list-item>
                </template>
              </v-list-item-group>
            </v-list>

            <!---
          <v-list subheader>
      <v-subheader>Recent chat</v-subheader>
      <v-list-item
        v-for="chat in recent"
        :key="chat.title"
      >
        <v-list-item-avatar>
          <v-img
            :alt="`${chat.title} avatar`"
            :src="chat.avatar"
          ></v-img>
        </v-list-item-avatar>

        <v-list-item-content>
          <v-list-item-title v-text="chat.title"></v-list-item-title>
        </v-list-item-content>

        <v-list-item-icon>
          <v-icon :color="chat.active ? 'deep-purple accent-4' : 'grey'">
            mdi-message-outline
          </v-icon>
        </v-list-item-icon>
      </v-list-item>
    </v-list>-->

            <!---<v-list-item active-class="deep-purple--text text--accent-4">
              <v-btn
                depressed
                @click="
                  dialogProduct = true
                  listId = list.id
                "
                class="mt-auto"
                >Build Product</v-btn
              >

              <v-dialog v-model="dialogProduct" persistent max-width="600px">
                <v-card elevation="0">
                  <v-card-title>
                    <span class="headline">Card name</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12">
                          <v-text-field
                            label="Stuff to do"
                            required
                          ></v-text-field>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                      color="blue darken-1"
                      text
                      @click="dialogProduct = false"
                    >
                      Close
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="dialogProduct()">
                      Save
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-list-item>

            <v-list-item
              active-class="deep-purple--text text--accent-4"
              :draggable="true"
              @dragover.prevent
            >
            <!---
              <template v-slot:default="{ active }">
                <v-list-item-content>
                  <v-list-item-title>Test</v-list-item-title>
                </v-list-item-content>

                <v-list-item-action>
                  <v-icon>mdi-delete-outline</v-icon>
                </v-list-item-action>

                <v-list-item-action>
                  <v-checkbox
                    :input-value="active"
                    color="deep-purple accent-4"
                  ></v-checkbox>
                </v-list-item-action>
              </template>
            </v-list-item>--->

            <v-divider></v-divider>
          </v-list-group>
        </v-card>

        <v-dialog v-model="dialogProduct" persistent max-width="600px">
          <v-card elevation="0">
            <v-card-title>
              <span class="headline">Card name</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      label="Add Sub Products"
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialogProduct = false">
                Close
              </v-btn>
              <v-btn color="blue darken-1" text @click="createProduct()">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-btn
          depressed
          @click="
            dialogCard = true
            listId = list.id
          "
          class="mt-auto"
          >Add card</v-btn
        >
      </div>
      <v-dialog v-model="dialogCard" persistent max-width="600px">
        <v-card elevation="0">
          <v-card-title>
            <span class="headline">Card name</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    label="Stuff to do"
                    v-model="card.title"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialogCard = false">
              Close
            </v-btn>
            <v-btn color="blue darken-1" text @click="createCard()">
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <div class="d-flex flex-row">
        <v-btn depressed @click="dialog = true" class="create-list"
          >Create new list</v-btn
        >
        <v-dialog v-model="dialog" persistent max-width="600px">
          <v-card elevation="0">
            <v-card-title>
              <span class="headline">List name</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      label="Stuff to do"
                      v-model="list.title"
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialog = false">
                Close
              </v-btn>
              <v-btn color="blue darken-1" text @click="createList()">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
      <v-dialog v-model="dialogEditCard" persistent max-width="600px">
        <v-card elevation="0">
          <v-card-title>
            <span class="headline">{{ currentCard.title }}</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    label="Edit title"
                    v-model="currentCard.title"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="red darken-1" text @click="deleteCard()">
              Delete
            </v-btn>
            <v-btn color="blue darken-1" text @click="dialogEditCard = false">
              Close
            </v-btn>
            <v-btn color="blue darken-1" text @click="updateCard()">
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'
export default {
  layout: 'board',
  data() {
    return {
      cardId: '',
      listId: '',
      list: {
        title: '',
      },
      card: {
        title: '',
      },
      product: {
        title: '',
      },

      recent: [
        {
          active: true,
          avatar: 'https://cdn.vuetifyjs.com/images/lists/1.jpg',
          title: 'Jason Oner',
        },
        {
          active: true,
          avatar: 'https://cdn.vuetifyjs.com/images/lists/2.jpg',
          title: 'Mike Carlson',
        },
        {
          avatar: 'https://cdn.vuetifyjs.com/images/lists/3.jpg',
          title: 'Cindy Baker',
        },
        {
          avatar: 'https://cdn.vuetifyjs.com/images/lists/4.jpg',
          title: 'Ali Connors',
        },
      ],
      currentCard: {},
      cardDraggedId: '',
      cardDraggedListId: '',
      dialog: false,
      dialogCard: false,
      dialogEditCard: false,
      dialogProduct: false,
      drawer: false,
    }
  },
  async asyncData({ params }) {
    //lets get our board data before page load, and then after that await changes
    let boardRef = $nuxt.$fire.firestore
      .collection('users')
      .doc($nuxt.$fire.auth.currentUser.uid)
      .collection('boards')
      .doc(params.id)
    let boardData = {}
    await boardRef
      .get()
      .then(function (doc) {
        if (doc.exists) {
          boardData = doc.data()
          boardData.id = params.id
        }
      })
      .catch(function (error) {})
    // if (boardData.color != '' || boardData.image.downloadURL != '') {
    //   $nuxt.$emit('toggle-alt-topbar')
    // }
    return { board: boardData }
  },
  created() {
    let that = this
    let tempId = this.board.id
    let boardRef = $nuxt.$fire.firestore
      .collection('users')
      .doc($nuxt.$fire.auth.currentUser.uid)
      .collection('boards')
      .doc(tempId)
      .onSnapshot((doc) => {
        if (doc.exists) {
          that.board = doc.data()
          that.board.id = tempId
        }
      })
  },
  methods: {
    async createList() {
      let that = this
      that.dialog = false
      if (that.list.title != '') {
        //add to firebase
        //Let's give our list a created date.
        that.list.id = uuidv4()
        that.list.cards = []
        // that.list.cards.product  = []
        that.list.dateCreated = Date.now()
        if (that.board.lists) {
          that.board.lists.push(that.list)
        } else {
          that.board.lists = []
          that.board.lists.push(that.list)
        }
        await that.updateBoard()
        that.list = {}
      }
    },
    async updateCardList(newlistId) {
      let that = this

      let tempListIndex = -1
      let tempCardIndex = -1
      let newListIndex = -1
      let tempListCount = 0
      let tempCardCount = 0

      //get the index in current cards current list
      for (const list of that.board.lists) {
        if (list.id === newlistId) {
          newListIndex = tempListCount
        }
        if (that.currentCard.listId === list.id) {
          //correct list, now find card
          tempListIndex = tempListCount
          for (const card of list.cards) {
            if (card.id === that.currentCard.id) {
              tempCardIndex = tempCardCount
            }
            tempCardCount++
          }
        }
        tempListCount++
      }

      //remove currentCard from current list
      that.board.lists[tempListIndex].cards.splice(tempCardIndex, 1)

      //add currentCard to its new list
      that.currentCard.listId = newlistId
      that.board.lists[newListIndex].cards.push(that.currentCard)

      await that.updateBoard()
    },
    allowDrop(ev) {
      ev.preventDefault()
    },
    drag(ev, card) {
      this.currentCard = card
    },
    drop(ev, listId) {
      ev.preventDefault()
      this.updateCardList(listId)
    },
    async deleteList(listId) {
      let that = this
      let index = -1
      let count = 0
      for (const list of that.board.lists) {
        if (list.id == listId) {
          index = count
        }
        count++
      }
      if (index > -1) {
        that.board.lists.splice(index, 1)
        await that.updateBoard()
      }
    },
    async createCard() {
      let that = this
      that.dialogCard = false
      //show modal to capture card name
      //add card
      if (that.card.title != '') {
        //add to firebase
        //Let's give our card a created date.
        that.card.id = uuidv4()
        that.card.product = []
        that.card.dateCreated = Date.now()
        that.card.listId = that.listId
        if (that.board.lists) {
          let index = -1
          let count = 0
          for (const list of that.board.lists) {
            if (list.id === that.listId) {
              index = count
            }
            count++
          }
          if (index != -1) {
            //we found the list, now push our card
            if (that.board.lists[index].cards) {
              that.board.lists[index].cards.push(that.card)
            } else {
              that.board.lists[index].cards = []
              that.board.lists[index].cards.push(that.card)
            }
          }
        }
        await that.updateBoard()
        that.card = {}
        that.listId = ''
      }
    },
    editCard(card) {
      this.dialogEditCard = true
      this.currentCard = card
    },
    async updateCard() {
      let that = this
      that.dialogEditCard = false
      for (const list of that.board.lists) {
        if (that.currentCard.listId === list.id) {
          //correct list, now find card
          for (const card of list.cards) {
            if (card.id === that.currentCard.id) {
              card = that.currentCard
            }
          }
        }
      }
      await that.updateBoard()
    },
    async deleteCard() {
      let that = this
      that.dialogEditCard = false
      let i = 0
      let j = 0
      let index = {
        list: -1,
        card: -1,
      }
      for (const list of that.board.lists) {
        if (that.currentCard.listId === list.id) {
          //correct list, now find card
          for (const card of list.cards) {
            if (card.id === that.currentCard.id) {
              index.list = i
              index.card = j
            }
            j++
          }
        }
        i++
      }
      if (index.list != -1) {
        //we found the list, now push our card
        if (that.board.lists[index.list].products) {
          that.board.lists[index.list].products.push(that.product)
        } else {
          that.board.lists[index.list].products = []
          that.board.lists[index.list].products.push(that.product)
        }
      }

      await that.updateBoard()
      that.product = {}
      that.productId = ''
    },

    async createProduct() {
      /*  let that = this
      that.dialogProduct = false
      //show modal to capture card name
      //add card
      if (that.product.title != '') {
        //add to firebase
        //Let's give our card a created date.
        that.product.id = uuidv4()
        that.product.product = []
        that.product.dateCreated = Date.now()
        that.product.cardId = that.cardId
        if (that.board.lists) {
          let i = 0
          let j = 0
          let index = {
            list: -1,
            card: -1,
          }
          for (const list of that.board.lists) {
            if (list.id === that.listId) {
              //correct list, now find card
              for (const card of list.cards) {
                if (card.id === that.card.id) {
                  index.list = i
                  index.card = j
                }
                j++
              }
            }
            i++
          }

          if (index.list > -1) {
            that.board.lists[index.list].cards.push(index.card, 1)
            await that.updateBoard()
          }
        }
      }*/
    },

    async deleteBoard() {
      let that = this
      try {
        await that.$fire.firestore
          .collection('users')
          .doc(that.$fire.auth.currentUser.uid)
          .collection('boards')
          .doc(that.board.id)
          .delete()
          .then(() => {
            $nuxt.$router.push('/')
          })
          .catch(() => {})
      } catch (error) {
        $nuxt.$router.push('/')
      }
    },
    async updateBoard() {
      let that = this
      await that.$fire.firestore
        .collection('users')
        .doc(that.$fire.auth.currentUser.uid)
        .collection('boards')
        .doc(that.board.id)
        .update(that.board, { merge: true })
    },
  },
}
</script>

<style lang="scss" scoped>
.board {
  padding: 12px;
  height: 100vh;
  overflow: scroll;
  .list {
    min-width: 350px;
    background-color: rgb(228 228 228 / 35%);
    padding: 25px;
    border-radius: 12px;
    min-height: 70vh;
  }
  .create-list {
    background-color: rgb(228 228 228 / 35%);
  }
  a {
    text-decoration: none;
  }
  .menu-items a {
    color: $text-color;
    padding: 10px 0px 10px 3px;
    font-size: 24px;
  }
  .jello-topbar {
    background-color: rgb(255, 255, 255, 0);
    padding: 0px !important;
  }
}
</style>
