<template>
    <div id="app" class="container">
        <div class="page-header">
            <h1> Vue.js 2 & Firebase Sample Application</h1>
        </div>

        <div class="panel panel-default">
            <div class="panel-heading">
                <h3>Add Book</h3>
            </div>  
            <div class="panel-body">
                <form id="form" class="form-inline" v-on:submit.prevent="addBook">
                    <div class="form-group">
                        <label for="bookTitle">Title:</label>
                        <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
                    </div>
                    <div class="form-group">
                        <label for="bookAuthor">Author:</label>
                        <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
                    </div>
                    <div class="form-group">
                        <label for="bookUrl">URL</label>
                        <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
                    </div>
                    <input type="submit" class="btn btn-primary" value="Add Book">
                </form>
            </div>
        </div> 
    
        <div class="panel panel-default">
            <div class="panel-heading">
                <h3>Books Lists</h3>
            </div>
            <div class="panel-body">
                <table class="table table-striped">
                    <thead>
                        <tr>
                            <th>
                                Title
                            </th>
                            <th>
                                Author
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="book in books">
                            <td>
                                <a v-bind:href="book.url">{{ book.title }}</a>
                            </td>
                            <td>
                                {{ book.author }}
                            </td>
                            <td>
                                <span class="glyphicon glyphicon-trash" v-on:click="rmBook(book)"></span>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <!--<img src="./assets/logo.png">-->
    </div>
</template>

<script>
import Firebase from 'firebase'
import toastr from 'toastr'

// configuration object to be passed to firebase service
// can be seen on console.firebase > overview > add firebase to ur webapp
let config = {
    apiKey: 'AIzaSyD0ouQ0_LrtBFNbrPZRaDXvnVOf24XUH80',
    authDomain: 'vuejs-firebase-01-48f56.firebaseapp.com',
    databaseURL: 'https://vuejs-firebase-01-48f56.firebaseio.com',
    projectId: 'vuejs-firebase-01-48f56',
    storageBucket: 'vuejs-firebase-01-48f56.appspot.com',
    messagingSenderId: '617960347754'
}

// initialize firebase application
let app = Firebase.initializeApp(config)
// can now access db https://vuejs-firebase-01-48f56.firebaseio.com/
let db = app.database()

// access to data stored inside 'books' node
let booksRef = db.ref('books');

// called as component configuration object
// export with firebase object. w/ prop of books and val of booksRef
export default {
    name: 'app',
    firebase: {
        books: booksRef
    },
    data () {
        return {
            newBook: {
                title: '',
                author: '',
                url: ''
            }
        }
    },
    methods: {
        addBook : function () {
            booksRef.push(this.newBook);
            toastr.success("Added " + this.newBook.title + " by " + this.newBook.author + " to Books List.");
            this.newBook.title = '';
            this.newBook.author = '';
            this.newBook.url = '';
        },
        rmBook : function(book) {
            console.log(book);
            booksRef.child(book['.key']).remove();
            toastr.success("Book " + book['title'] + " by " + book['author'] + " is removed.");
        }
    }
}
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
