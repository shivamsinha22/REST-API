# REST-API
🔧 Step-by-Step Instructions
✅ 1. Initialize Project
Open terminal and run:


mkdir book-api
cd book-api
npm init -y
✅ 2. Install Express

npm install express
✅ 3. Setup Basic Express Server (port 3000)
Create a file named server.js:


const express = require('express');
const app = express();
const PORT = 3000;

// Middleware to parse JSON body
app.use(express.json());

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
✅ 4. Create an Array to Store Book Objects
Add this inside server.js:


let books = []; // In-memory store
let nextId = 1; // Incremental ID generator
Each book will look like:


{
  "id": 1,
  "title": "Book Title",
  "author": "Author Name"
}
✅ 5. Implement GET /books to Return All Books

app.get('/books', (req, res) => {
  res.json(books);
});
✅ 6. Implement POST /books to Add a New Book

app.post('/books', (req, res) => {
  const { title, author } = req.body;

  if (!title || !author) {
    return res.status(400).json({ message: 'Title and author are required' });
  }

  const newBook = { id: nextId++, title, author };
  books.push(newBook);
  res.status(201).json(newBook);
});
✅ 7. Implement PUT /books/:id to Update a Book by ID

app.put('/books/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const { title, author } = req.body;
  const book = books.find(b => b.id === id);

  if (!book) {
    return res.status(404).json({ message: 'Book not found' });
  }

  if (title) book.title = title;
  if (author) book.author = author;

  res.json(book);
});
✅ 8. Implement DELETE /books/:id to Remove a Book

app.delete('/books/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = books.findIndex(b => b.id === id);

  if (index === -1) {
    return res.status(404).json({ message: 'Book not found' });
  }

  const deleted = books.splice(index, 1)[0];
  res.json(deleted);
});
✅ 9. Test Endpoints in Postman
Method	Endpoint	Description	Body Example (JSON)
GET	/books	Get all books	–
POST	/books	Add a book	{ "title": "1984", "author": "Orwell" }
PUT	/books/1	Update book by ID	{ "title": "Animal Farm" }
DELETE	/books/1	Delete book by ID	–

✅ Final server.js Code (Complete)

const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json());

// In-memory storage
let books = [];
let nextId = 1;

// GET all books
app.get('/books', (req, res) => {
  res.json(books);
});

// POST a new book
app.post('/books', (req, res) => {
  const { title, author } = req.body;
  if (!title || !author) {
    return res.status(400).json({ message: 'Title and author are required' });
  }
  const newBook = { id: nextId++, title, author };
  books.push(newBook);
  res.status(201).json(newBook);
});

// PUT update a book
app.put('/books/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const { title, author } = req.body;
  const book = books.find(b => b.id === id);
  if (!book) {
    return res.status(404).json({ message: 'Book not found' });
  }
  if (title) book.title = title;
  if (author) book.author = author;
  res.json(book);
});

// DELETE a book
app.delete('/books/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = books.findIndex(b => b.id === id);
  if (index === -1) {
    return res.status(404).json({ message: 'Book not found' });
  }
  const deleted = books.splice(index, 1)[0];
  res.json(deleted);
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
