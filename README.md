<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Grāmatu bibliotēka</title>
    <style>
        body {
            font-family: Arial;
            max-width: 600px;
            margin: auto;
            padding: 20px;
        }
        input, button {
            margin: 5px 0;
            padding: 8px;
            width: 100%;
        }
        .book {
            border: 1px solid #ccc;
            padding: 10px;
            margin-top: 10px;
        }
        .read {
            background-color: #d4edda;
        }
    </style>
</head>
<body>

<h1>📚 Grāmatu bibliotēka</h1>

<input type="text" id="title" placeholder="Grāmatas nosaukums">
<input type="text" id="author" placeholder="Autors">
<input type="number" id="year" placeholder="Gads">

<button onclick="addBook()">Pievienot grāmatu</button>

<h2>Grāmatu saraksts:</h2>
<div id="bookList"></div>

<script>
let books = JSON.parse(localStorage.getItem("books")) || [];

function saveBooks() {
    localStorage.setItem("books", JSON.stringify(books));
}

function addBook() {
    let title = document.getElementById("title").value.trim();
    let author = document.getElementById("author").value.trim();
    let year = document.getElementById("year").value.trim();
    
 if (!title || !author || !year) {
        alert("Aizpildiet visus laukus!");
        return;
    }

    books.push({
        id: Date.now(),
        title: title,
        author: author,
        year: year,
        read: false
    });

    saveBooks();
    renderBooks();

    document.getElementById("title").value = "";
    document.getElementById("author").value = "";
    document.getElementById("year").value = "";
}

function deleteBook(id) {
    books = books.filter(book => book.id !== id);
    saveBooks();
    renderBooks();
}

function toggleRead(id) {
    books = books.map(book => {
        if (book.id === id) {
            book.read = !book.read;
        }
        return book;
    });
    saveBooks();
    renderBooks();
}

function renderBooks() {
    let list = document.getElementById("bookList");
    list.innerHTML = "";

    books.forEach(book => {
        let div = document.createElement("div");
        div.className = "book" + (book.read ? " read" : "");

        div.innerHTML = `
            <strong>${book.title}</strong><br>
            Autors: ${book.author}<br>
            Gads: ${book.year}<br><br>
            <button onclick="toggleRead(${book.id})">
                ${book.read ? "Lasīt" : "Nav lasīts"}
            </button>
            <button onclick="deleteBook(${book.id})">Dzēst</button>
        `;

        list.appendChild(div);
    });
}

renderBooks();
</script>

</body>
</html>
