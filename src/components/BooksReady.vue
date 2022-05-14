<template>
  <div class="nav-bar">
    <p>Lorem Board</p>
    <p>books of</p>
    <input v-model="authorSearchID" placeholder="Type author ID...">
    <button @click="getBooks">Submit</button>
    <h4 v-if="loadComplete == false">Books are on their way :)</h4>
  </div>
  <div class="container">
    <section class="books">
      <GetBooks :publishYears="division1" :allBooks="allBooks"/>
    </section>
    <section class="books">
      <GetBooks :publishYears="division2" :allBooks="allBooks"/>
    </section>
    <section class="books">
      <GetBooks :publishYears="division3" :allBooks="allBooks"/>
    </section>
    <section class="books">
      <GetBooks :publishYears="division4" :allBooks="allBooks"/>
    </section>
  </div>

</template>

<script>
import GetBooks from './GetBooks.vue';
export default {
    name: "BooksReady",
    methods: {
      getBooks() {
            this.loadComplete = false;
            fetch(`http://openlibrary.org/search.json?author=${this.authorSearchID}`).then(data => data.json())
                .then(response => {
                // OL26320A
                this.loadComplete = true;
                let unsplittedYears = [];
                let allBooks = [];
                let publishYears = [];
                for (let prop of response.docs) {
                    this.parsePackage(prop,allBooks,unsplittedYears);
                }
                unsplittedYears = unsplittedYears.sort();
                this.splitArray(unsplittedYears,publishYears);
                this.division1 = publishYears[0];
                this.division2 = publishYears[1];
                this.division3 = publishYears[2];
                this.division4 = publishYears[3];
                this.allBooks = allBooks;
            });
        },
        parsePackage(book,bookArr,publishArr){
        // Making the book object with details below
        if (book.language !== undefined) {
          if (book.language.includes("eng")){
              let maxNum = 0;
              for (let i = 0; i < book.publish_year.length; i++) {
                  if (book.publish_year[i] > maxNum) {
                      maxNum = book.publish_year[i];
                  }
              }
              if (!publishArr.includes(maxNum)) {
                  publishArr.push(maxNum);
              }  
              let aBook = new Object();
              aBook.title = book.title;
              aBook.latest = maxNum;
              aBook.firstYear = book.first_publish_year;
              aBook.editions = book.edition_count;
              aBook.author = book.author_name[0];
              bookArr.push(aBook);
            }
        }
        },
        splitArray(oldArr,newArr){
          let splitCount = Math.ceil(oldArr.length / 4);
          while(oldArr.length>0){
            newArr.push(oldArr.splice(0,splitCount));
          }
        }
    },
    data: () => ({
        division1: null,
        division2: null,
        division3: null,
        division4: null,
        allBooks: undefined,
        loadComplete: undefined,
        authorSearchID: ''
    }),
    components: { GetBooks }
}
</script>
<style lang="scss">
$base-display: flex;
$container-direction: row;
$slight-margin: 0.75rem;
$row-display-width: 25%;
$centered: center;
// Kanban Boards Variabless
$kanban-direction: column;
$slight-border-radius: 4px;
$board-box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
$board-height: 30vh;
$no-overflow: hidden;
// Book Caps Variables
$book-caps-backColor: rgb(230, 227, 227);
$edition-backColor: rgba(0, 163, 0, 0.623);

.nav-bar{
  display: $base-display;
  flex-direction: $container-direction;
  height: 5vh;
  justify-content: $centered;
  align-items: $centered;
  * {
    margin-right: $slight-margin;
  }
  p:first-child{
    font-weight: bolder;
    font-size:x-large;
    font-family: 'Times New Roman', Times, serif;
  }
  small{
    margin-left: 2em;
  }
  input {
    height: 2vh;
  }
  button {
    background-color: black;
    color: rgba(255, 255, 255, 0.712);
    border-radius: $slight-border-radius;
  }
}
.container {
    display: $base-display;
    flex-direction: $container-direction;
    margin: $slight-margin;
}
.books {
    display: $base-display;
    flex-direction: $kanban-direction;
    margin-right: 3em;
    width: $row-display-width;
}
.kanban-board-hidden {
    border: 1px solid black;
    box-shadow: $board-box-shadow;
    margin-bottom: $slight-margin;
    border-radius: $slight-border-radius;
    height: $board-height;
    overflow: $no-overflow;
    transition: 2s;
    button {
      background-color: transparent;
      border-radius: $slight-border-radius;
    }
    button:active {
      background-color: yellow;
    }
}
.kanban-board-visible{
    border: 1px solid black;
    box-shadow: $board-box-shadow;
    margin-bottom: $slight-margin;
    border-radius: $slight-border-radius;
    height: auto;
    overflow: auto;
    transition: 2s;
    button {
      background-color: transparent;
    }
    button:active {
      background-color: yellow;
    }
}
.book-caps{
    background-color: $book-caps-backColor;
    border-radius: $slight-border-radius;
    margin: $slight-margin;
    justify-content: $centered;
    align-items: $centered;
    text-align: $centered;
}
.edits-published {
    display: $base-display;
    flex-direction: $container-direction;
    justify-content: $centered;
    align-items: $centered;

    p:first-child {
    margin: $slight-margin;
    background-color: $edition-backColor;
    color: white;
    padding: 0 0.5rem 0 0.5rem;
    border-radius: $slight-border-radius;
    }
}

</style>
