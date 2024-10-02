import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

class EditBookScreen extends StatelessWidget {
  final String bookId;

  const EditBookScreen({Key? key, required this.bookId}) : super(key: key);

  Future<Map<String, dynamic>> _fetchBookData(String bookId) async {
    var response =
        await http.get(Uri.parse('http://localhost:3000/books/${bookId}'));
    if (response.statusCode == 200) {
      return jsonDecode(response.body);
    } else {
      throw Exception('Failed to load book data');
    }
  }

  Future<void> _updateBook(
      String bookId, Map<String, dynamic> updatedData) async {
    var url = Uri.parse('http://localhost:3000/books/$bookId');
    var response = await http.put(
      url,
      headers: {
        'Content-Type': 'application/json',
      },
      body: jsonEncode(updatedData),
    );

    if (response.statusCode == 200) {
      print('Book updated successfully');
    } else {
      print('Failed to update book');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        centerTitle: true,
        title: const Text(
          'EDIT BOOK',
          style: TextStyle(
            fontSize: 24,
            fontWeight: FontWeight.bold,
            color: Colors.black,
          ),
        ),
        backgroundColor: const Color.fromARGB(255, 255, 217, 92),
        iconTheme: const IconThemeData(color: Colors.black),
      ),
      body: FutureBuilder<Map<String, dynamic>>(
        future: _fetchBookData(bookId),
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('Error: ${snapshot.error}'));
          } else if (!snapshot.hasData) {
            return const Center(child: Text('No data found'));
          }

          var bookData = snapshot.data!;
          String? _bookName = bookData['title'];
          String? _authorName = bookData['author'];
          String? _category = bookData['category'];
          String? _yearOfPublication = bookData['yearpubish'];
          String? _isbn = bookData['isbn'];

          final _formKey = GlobalKey<FormState>();

          return Form(
            key: _formKey,
            child: SingleChildScrollView(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  TextFormField(
                    initialValue: _bookName,
                    decoration: const InputDecoration(labelText: "Book Name"),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter a book name';
                      }
                      return null;
                    },
                    onSaved: (value) {
                      _bookName = value;
                    },
                  ),
                  TextFormField(
                    initialValue: _authorName,
                    decoration: const InputDecoration(labelText: "Author Name"),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter an author name';
                      }
                      return null;
                    },
                    onSaved: (value) {
                      _authorName = value;
                    },
                  ),
                  DropdownButtonFormField<String>(
                    value: _category,
                    decoration: const InputDecoration(labelText: "Category"),
                    items: <String>['Fantasy', 'Romance', 'Mystery']
                        .map((String value) {
                      return DropdownMenuItem<String>(
                        value: value,
                        child: Text(value),
                      );
                    }).toList(),
                    onChanged: (String? newValue) {
                      _category = newValue;
                    },
                    onSaved: (value) {
                      _category = value;
                    },
                  ),
                  TextFormField(
                    initialValue: _yearOfPublication,
                    decoration:
                        const InputDecoration(labelText: "Year of Publication"),
                    onSaved: (value) {
                      _yearOfPublication = value;
                    },
                  ),
                  TextFormField(
                    initialValue: _isbn,
                    decoration: const InputDecoration(labelText: "ISBN"),
                    onSaved: (value) {
                      _isbn = value;
                    },
                  ),
                  const SizedBox(height: 20),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceAround,
                    children: [
                      ElevatedButton(
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.blue,
                          padding: const EdgeInsets.symmetric(
                              horizontal: 30, vertical: 15),
                        ),
                        child: const Text(
                          "Save",
                          style: TextStyle(color: Colors.white),
                        ),
                        onPressed: () {
                          if (_formKey.currentState!.validate()) {
                            _formKey.currentState!.save();
                            var updatedBook = {
                              'title': _bookName,
                              'author': _authorName,
                              'category': _category,
                              'yearpubish': _yearOfPublication,
                              'isbn': _isbn,
                            };
                            _updateBook(bookId, updatedBook).then((_) {
                              Navigator.pop(context);
                            });
                          }
                        },
                      ),
                      ElevatedButton(
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.red,
                          padding: const EdgeInsets.symmetric(
                              horizontal: 30, vertical: 15),
                        ),
                        child: const Text(
                          "Cancel",
                          style: TextStyle(color: Colors.white),
                        ),
                        onPressed: () {
                          Navigator.pop(context);
                        },
                      ),
                    ],
                  ),
                ],
              ),
            ),
          );
        },
      ),
    );
  }
}
