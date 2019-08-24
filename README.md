# Vue File Picker

## Install
```bash
npm install vue-file-picker --save
```
or 
```bash
yarn add vue-file-picker
```

In your ```main.js``` file, add
```javascript
import VueFilePicker from 'vue-file-picker'
Vue.use(VueFilePicker)
```
This will register the ```<vue-file-picker /> ``` component. 

## Usage
In your component, 
```html
<form @submit="submit">
  <div class="form-group">
    <label>File</label>
    <vue-file-picker v-model="image" :multiple="true" @change="test" />
  </div>
</form>
```
```javascript
export default {
  data() {
    return {
      images: ''
    }
  },

  methods: {
    test() {
      console.log(this.images);
    }
  }
}
```
This renders

![Vue Image Manager Demo](demo/1.png)

and upon file selection

![Vue Image Manager Demo](demo/2.png)

and the ```console.log(this.images)``` results in an array cotaining the 2 images selected as file objects. For multiple="false" or single file input, the result is the single object that is the file. Therefore, the data is ready to be multipart/form-data encoded and sent to a server, no further procesing required.
