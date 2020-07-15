<template>
  <div class="row">
    <div class="col-3">
      <h3>user</h3>
      <draggable class="list-group" :list="users" group="people" @change="log">
        <div
          class="list-group-item"
          v-for="(element) in users"
          :key="element.id"
        >
          {{ element.name }} 
        </div>
      </draggable>
    </div>

    <div class="col-3">
      <h3>Images</h3>
      <draggable class="list-group" @remove="removeFromImages" :list="images" group="people" @add="addToImages" >
        <div
          class="list-group-item"
          v-for="(element, index) in images"
          :key="element.id"
        >
        <img v-if=" index<images.length && element.image != ''" :src="require('@/assets/'+element.image)" style="height:50px;width:50px;">
        {{ element.name }} 
        </div>
      </draggable>
    </div>

    <!-- <rawDisplayer class="col-3" :value="users" title="List 1" />

    <rawDisplayer class="col-3" :value="images" title="List 2" /> -->
  </div>
</template>
<script>
  import draggable from 'vuedraggable';
export default {
  name: "TwoLists",
  order: 1,
  components: {
    draggable
  },
  data() {
    return {
      users: [
        { name: "John", id: 1, image:"" },
        { name: "Joao", id: 2, image:"" },
        { name: "Anna", id: 3, image:"" },
        { name: "Mike", id: 4, image:"" },
      ],
      images: [
        { name: "", id: 5, image:"home.png" },
        { name: "", id: 6, image:"avatar.png" },
      ],
    };
  },
  methods: {
    addToImages(evt) {
      if(evt.newDraggableIndex == this.images.length-1){
        this.removeFromList(evt.oldDraggableIndex, evt.newDraggableIndex);
      }else{
         let bottomImage = this.images[evt.newDraggableIndex+1].image; 
         let count= 0;
        this.images.forEach(object => {
          if(object.image == bottomImage && object.name == ""){
            count++;
          }
        });
        if(count==1 && bottomImage!=""){
          this.images[evt.newDraggableIndex].image = bottomImage;
          this.images.splice(evt.newDraggableIndex+1, 1);
        }else{
            this.removeFromList(evt.oldDraggableIndex, evt.newDraggableIndex);
        }
          
      }
    },

    removeFromList(oldDraggableIndex, newDraggableIndex){
      let object = this.images[newDraggableIndex];
      this.images.splice(newDraggableIndex, 1);
      this.users.splice(oldDraggableIndex, 0, object);
    },

    removeFromImages(evt){
      let object = this.users[evt.newDraggableIndex];
      this.users.splice(evt.newDraggableIndex, 1);
      this.images.splice(evt.oldDraggableIndex, 0, object);
    },

    log(evt) {
      window.console.log(evt);
    },
  }
};
</script>