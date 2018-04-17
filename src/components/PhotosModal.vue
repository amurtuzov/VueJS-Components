<template>
	<transition name="modal">
    	<div class="modal-mask">
      		<div class="modal-wrapper">
        		<div class="modal-container">
          			<div class="modal-header">
              			Album: {{albumName}}
          			</div>
          			<div class="modal-body">
              			<div v-for="photo in lazyLoadPhotos">
              				<img v-bind:src="photo.thumbnailUrl"/>
              			</div>
          			</div>
          			<div class="modal-footer">
              			<button class="modal-default-button close-btn" @click="$emit('close')" v-on:click="closeModal">Close</button>
              			<button class="modal-default-button loadPhotos-btn" @click="getMorePhotos">More photos</button>
          			</div>
        		</div>
      		</div>
    	</div>
  </transition>
</template>

<script>
  export default {
      data () {
    return {
      photosCounter: 10
    }
  },
    props: {
      lazyLoadPhotos: {
        type: Array
      },
      photos: {
        type: Array
      },
      showModal: {
        type: Boolean
      },
      albumName: {
        type: String
      }
    },
    methods: {
      closeModal: function() {
        this.photosCounter = 10;
        while(this.lazyLoadPhotos.length > 0) {this.lazyLoadPhotos.pop();}
      },
      getMorePhotos: function() {
      this.photosCounter = this.photosCounter + 10;
      if (this.lazyLoadPhotos.length < 50) {
        for (var i = 0; i < this.photosCounter; i++) {
          if (this.lazyLoadPhotos.indexOf(this.photos[i]) === -1) {
            this.lazyLoadPhotos.push(this.photos[i]);
            }
        }
      }
    },
    }
	}
</script>

<style>
	.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0; 
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, .5);
  display: table;
  transition: opacity .3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
  height: 100vh;
}

.modal-container {
  width: 800px;
  height: 500px;
  overflow-y: scroll;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
}

/*.modal-default-button {
  float: right;
}*/
.modal-footer {
  display: flex;
  flex-direction: column-reverse;
}
.modal-footer .loadPhotos-btn {
  width: 100px;
  margin: auto;
}
.modal-footer .close-btn {
  width: 50px;
  margin-left: auto;
  margin-top: 20px;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>