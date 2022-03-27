<template >
	<section class="container pt-5 mb-5 pb-5">
	<div class="d-flex">
	<div class="side-left ">
	<div class="d-flex flex-column flex-shrink-0 p-3 bg-light" style="width: 280px;">

    <h2 class=" text-center">
     BOOKCreator
 </h2>
   <input v-model="titlebook" placeholder="Название книги" type="text" class="form-control">
	
   
	<hr>


	<sidebar-items :postitems="posts" @selectitem="Select" @additem="AddToItem" />
    <hr>


	<div v-if="posts.length > 0">
    <a href="#" class="card-link" @click="sel = null">Закрыть</a> <br>
 	<ul v-for="newp in posts" :key="newp"></ul>
	</div>

 

	<span v-else>Записей в книге нет!</span>
	<br>

	 <input type="file" class="form-control" id="selectFiles" v-on:keyup.enter="UploadBook"  />
<button id="import" class="btn btn-dark" @click="UploadBook">Импортировать книгу</button>
<br>
	<button @click="ExportBook" class="btn btn-dark">Экспортировать книгу </button> <br>
<button @click="SaveFile" class="btn btn-dark">Сохранить книгу </button>  <br>
<button @click="ExportBookHtml" class="btn btn-dark">Скачать книгу</button>


  </div>
  </div>

<div class="side-center ">
<div class="border p-3" style="width: 1000px; height:100%;" v-if="sel">
<h5 class="card-title">
  
<input type="text" class="form-control" style="width: 100%;" v-model="sel.title">

</h5>

<!-- <convert /> -->
<tiptap  :value="sel.body" @input="In" />


</div>




</div>
</div>
</section>
</template>


<script>
import Tiptap from '@/components/Tiptap.vue'
import SidebarItems from '@/components/SidebarItems.vue'
import Convert from '@/components/Convert.vue'
import jsPDF from 'jspdf'

export default {
	components: { Tiptap,SidebarItems, Convert  },
	data(){
		return{
			titlebook: '',
			posts: [],
			sel: null,		
      cposts: []
		
		}
},
created: function() {
       
    },
mounted() {
	
	if(localStorage.posts){
		this.posts = JSON.parse(localStorage.getItem("posts")) || []
    this.titlebook = localStorage.getItem("titlebook")

	} 
	
	
  },
  watch: {
   
	
  },
  computed: {
	  
  },
  
	

methods:{
  createPDF(){
    let pdfName = 'test'; 
    var doc = new jsPDF();
    doc.text(this.sel.body, 10, 10);
    doc.save(pdfName + '.pdf');
  },
	Select(post){
	this.sel = post; 
	},

  In(t){
	console.log(t);
 this.sel.body = t; 
	},

 SaveFile(){
	
	const parsed = JSON.stringify(this.posts);
      localStorage.setItem('posts', parsed);
	localStorage.setItem('titlebook', this.titlebook);
	console.log('ff');
	},

AddToItem(newitem){
const item = {     // объект
  title: newitem, 
  body: " "        
};
this.posts.push(item);
},

ExportBook(){
console.log(this.posts);

const data = JSON.stringify(this.posts)
    const blob = new Blob([data], {type: 'text/plain'})
    const e = document.createEvent('MouseEvents'),
    a = document.createElement('a');
    a.download = `${this.titlebook}.json`;
    a.href = window.URL.createObjectURL(blob);
    a.dataset.downloadurl = ['text/json', a.download, a.href].join(':');
    e.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
    a.dispatchEvent(e);

},


ExportBookHtml(){

  console.log(this.posts);
  var fff = [];
  var linksarr = [];
  for (let index = 0; index < this.posts.length; index++) {
    const headers = this.posts[index].title;
    const contents = this.posts[index].body;
  var ggg = `
<div class="bookbody">

   
    
    <center><b>${headers}</b></center> <br> <div id="${index}" >${contents} <div>
   
</div>

  `
  var links = `
  <a href="#${index}">${headers}</a>
  `
  linksarr.push(links);
fff.push(ggg)
  
  }



 var gg = `
 <div>
    <div>
    
        <label class="btn hb" for="modal-1" style=" text-align: left !importnat;">Оглавление книги</label>
      
    </div>
    
    
    <input class="modal-state" id="modal-1" type="checkbox" />
    <div class="modal">
      <label class="modal__bg" for="modal-1"></label>
      <div class="modal__inner">
          ${linksarr.join(' ' + '<br>')}
          </div>
    </div>
  </div>

<style>
    .bookbody{
        font-family: open-sans,sans-serif; font-size: 15px; padding:10px;
    }
 /* [Object] Modal
 * =============================== */
.modal {
  opacity: 0;
  visibility: hidden;
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
 width: 300px;
  text-align: left;
  background: rgba(0,0,0, .9);
  transition: opacity .25s ease;
}

.modal__bg {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  cursor: pointer;
}

.modal-state {
  display: none;
}

.modal-state:checked + .modal {
  opacity: 1;
  visibility: visible;
}

.modal-state:checked + .modal .modal__inner {
  top: 0;
}

.modal__inner {
  transition: top .25s ease;
  position: absolute;
  top: -20%;
  right: 0;
  bottom: 0;
  left: 0;
  width: 50%;
  margin: auto;
  overflow: auto;
  background: #fff;
  border-radius: 5px;
  padding: 1em 2em;
  height: 50%;
}

.modal__close {
  position: absolute;
  right: 1em;
  top: 1em;
  width: 1.1em;
  height: 1.1em;
  cursor: pointer;
}

.modal__close:after,
.modal__close:before {
  content: '';
  position: absolute;
  width: 2px;
  height: 1.5em;
  background: #ccc;
  display: block;
  transform: rotate(45deg);
  left: 50%;
  margin: -3px 0 0 -1px;
  top: 0;
}

.modal__close:hover:after,
.modal__close:hover:before {
  background: #aaa;
}

.modal__close:before {
  transform: rotate(-45deg);
}

@media screen and (max-width: 768px) {
	
  .modal__inner {
    width: 90%;
    height: 90%;
    box-sizing: border-box;
  }
}


/* Other
 * =============================== */
body {
  padding: 1%;
  font: 1/1.5em sans-serif;
  text-align: center;
}

.btn {
  cursor: pointer;
  background: #27ae60;

  padding: .5em 1em;
  color: #fff;
  border-radius: 3px;
}

.btn:hover,
.btn:focus {
  background: #2ecc71;
}

.btn:active {
  background: #27ae60;
  box-shadow: 0 1px 2px rgba(0,0,0, .2) inset;
}

.btn--blue {
  background: #2980b9;
}

.btn--blue:hover,
.btn--blue:focus {
  background: #3498db;
}

.btn--blue:active {
  background: #2980b9;
}


</style>




  `
fff.unshift(gg);

  console.log(fff);


const data = fff.join('');
    const blob = new Blob([data], {type: 'text/plain'})
    const e = document.createEvent('MouseEvents'),
    a = document.createElement('a');
    a.download = `${this.titlebook}.html`;
    a.href = window.URL.createObjectURL(blob);
    a.dataset.downloadurl = ['text/json', a.download, a.href].join(':');
    e.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
    a.dispatchEvent(e);

},
UploadBook(){


	
    var files = document.getElementById('selectFiles').files;
  console.log(files);
  if (files.length <= 0) {
    return false;
  }

  var fr = new FileReader();

  fr.onload = function(e) { 
  console.log(e);
    var result = JSON.parse(e.target.result);
    var formatted = JSON.stringify(result, null, 2);
		localStorage.setItem('posts', formatted);
    localStorage.removeItem('titlebook');
	window.location.reload();

		
  }

  fr.readAsText(files.item(0));

}


},

  
}
</script>

<style scoped>

</style>
