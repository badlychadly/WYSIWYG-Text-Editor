# WYSIWYG-Text-Editor

This past week I was working on a blogging app and relized something major, I had no clue on how I could enable blog features like headings and code blocks from the web page. How the heck do these sites like meduim enable a user to write and edit blogs with as many code snippets and headings and links as they want?? After a little research I stumbled upon something called a WYSIWYG(What you see is what you get) Text Editor. This can give the user the ability to write content directly inside an online app page. This is totally new to me but I want to share what I used and how I did it.

### CKEditor
The are a lot of options to use out there but I went with one that seemed fairly new and well maintained. I chose [CKEditor](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/integration/frameworks/react.html) because they had a React package and I could choose the type of editor I wanted. The [docs](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/integration/frameworks/react.html#quick-start) cover how to get started, but i'll go through it as well. Use npm to install the package. 
```
npm install --save @ckeditor/ckeditor5-react @ckeditor/ckeditor5-build-classic
```
This will install the classic build which comes with a toolbar and grows automatically with the content. The editor choices are BalloonEditor, ClassicEditor, DeoupledEditor, and InlineEditor.

The docs also provide this basic layout for the CKEditor component.
```
import React, { Component } from 'react';
import CKEditor from '@ckeditor/ckeditor5-react';
import ClassicEditor from '@ckeditor/ckeditor5-build-classic';

class App extends Component {
    render() {
        return (
            <div className="App">
                <h2>Using CKEditor 5 build in React</h2>
                <CKEditor
                    editor={ ClassicEditor }
                    data="<p>Hello from CKEditor 5!</p>"
                    onInit={ editor => {
                        // You can store the "editor" and use when it is needed.
                        console.log( 'Editor is ready to use!', editor );
                    } }
                    onChange={ ( event, editor ) => {
                        const data = editor.getData();
                        console.log( { event, editor, data } );
                    } }
                    onBlur={ editor => {
                        console.log( 'Blur.', editor );
                    } }
                    onFocus={ editor => {
                        console.log( 'Focus.', editor );
                    } }
                />
            </div>
        );
    }
}

export default App;
```
Here we have some basic properties for the component such as `editor`, `data`, and `onInit`. The `data` property will be displayed inside your web page text editor, but you can still keep track of it in local state. Take a look at the `onChange` property, notice that is has two parameters `(event, editor)`. Both of these are objects with lots of methods. To get the data that was written on the web page we call the `editor.getData()` method which returns a string of HTML. We can simplify the bulk of code in onChange by creating a `handleOnChange` prototype method in our App component.

```
state = {data: ""}

handleOnChange = (event, editor) => {
        const data = editor.getData();
            this.setState({data})
            console.log( { event, editor, data } );
    }
```
The onChange property for the CKEditor component can look like this `onChange={ this.handleOnChange }`, and you can change data to display state `data={this.state.data}.

Like I said at the beginning this is very new to me, but I wanted to share my approach. Be sure to check out the [CKEditor for React](https://ckeditor.com/docs/ckeditor5/latest/builds/guides/integration/frameworks/react.html#quick-start) documentation as it can give you a much deeper understanding. This is the way I chose to handle web page editing for a blog but I know there are many other ways that may be better for the task.
