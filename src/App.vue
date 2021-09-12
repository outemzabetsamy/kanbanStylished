<template>
  <div id="app">
    <Header/>
    <div class="board">
      <div class="lane">
        <h2 class="lane-title">
          TO DO
        </h2>
        <Container group-name="trello" @drag-start="handleDragStart('todo',$event)" 
        @drop="handleDrop('todo',$event)"
        :get-child-payload="getChildPayload"
        :drop-placeholder="{className:'placeholder'}" >
          <Draggable v-for="card in cards.todo" :key="card.id">
        <Card >
          
          <div :class="['item','todo']">
            <div class="kanban-card"> 
              <span class="card">
                <div class="kanban-action">
                  <label for=""><strong>#33</strong></label>
                  <button :class="['kanban-button','todo']">
                    <span>TO DO</span>
                  </button>
                </div>
                   <label class="label">Lorem ipsum dolor sit amet consectetur adipisicing elit. Libero, amet!</label>
              <small> Due date: 07/09/2021 </small>
              </span>
            </div>
          </div>
          <!--{{card.text}}-->
        </Card>
        </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">
          In Progress
        </h2>
        <Container group-name="trello" @drag-start="handleDragStart('inprogress',$event)" 
        @drop="handleDrop('inprogress',$event)"
        :get-child-payload="getChildPayload"
        :drop-placeholder="{className:'placeholder'}">
          <Draggable v-for="card in cards.inprogress" :key="card.id">
        <Card >
          <div :class="['item','inprogress']">
            <div class="kanban-card"> 
              <span class="card">
                <div class="kanban-action">
                  <label for=""><strong>#33</strong></label>
                  <button :class="['kanban-button','inprogress']">
                    <span>InProgress</span>
                  </button>
                </div>
                   <label class="label"> Lorem ipsum dolor, sit amet consectetur adipisicing elit. Repellendus, consequuntur!</label>
              <small> Due date: 07/09/2021 </small>
              </span>
            </div>
          </div>
        </Card>
        </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">
          Done
        </h2>
        <Container group-name="trello" @drag-start="handleDragStart('done',$event)" 
        @drop="handleDrop('done',$event)"
        :get-child-payload="getChildPayload">
          <Draggable v-for="card in cards.done" :key="card.id"
          :drop-placeholder="{className:'placeholder'}">
        <Card >
         <Card >
          <div :class="['item','done']">
            <div class="kanban-card"> 
              <span class="card">
                <div class="kanban-action">
                  <label for=""><strong>#33</strong></label>
                  <button :class="['kanban-button','done']">
                    <span>DONE</span>
                  </button>
                </div>
                   <label class="label"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Quam, temporibus.</label>
              <small> Due date: 07/09/2021 </small>
              </span>
            </div>
          </div>
        </Card>
        </Card>
        </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">
          Cancelled
        </h2>
         <Container group-name="trello" @drag-start="handleDragStart('cancelled',$event)" 
        @drop="handleDrop('cancelled',$event)"
        :get-child-payload="getChildPayload">
          <Draggable v-for="card in cards.cancelled" :key="card.id"
          :drop-placeholder="{className:'placeholder'}">
        <Card >
          <Card >
          <div :class="['item','cancelled']">
            <div class="kanban-card"> 
              <span class="card">
                <div class="kanban-action">
                  <label for=""><strong>#33</strong></label>
                  <button :class="['kanban-button','cancelled']">
                    <span>CANCELLED</span>
                  </button>
                </div>
                   <label class="label"> Lorem ipsum dolor sit amet consectetur adipisicing elit. Labore, neque!</label>
              <small> Due date: 07/09/2021 </small>
              </span>
            </div>
          </div>
        </Card>
        </Card>
        </Draggable>
        </Container>
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Card from './components/Card.vue'
import initialCards from "./initialCards"
import {Container,Draggable} from "vue-smooth-dnd"
export default {
  name: 'App',
  components: {
    Header,
    Card,
    Container,
    Draggable
  },
  data: ()=>({
    cards:{
      todo: initialCards.todo,
      inprogress: initialCards.inprogress,
      done: initialCards.done,
      cancelled: initialCards.cancelled,
      draggingCard: {
        lane: '',
        index:-1,
        cardData: {},
      }

    },
    
  }),
  methods: {
      handleDragStart(lane,dragResult){
       const { payload,isSource } = dragResult;
       console.log(lane);
       console.log(payload)
       console.log(isSource)
      //console.log(dragResult)
      if(isSource){
        this.draggingCard={
          index: payload,
          lane:lane,
          cardData: {
            ...this.cards[lane][payload]
          }
        }
      }
      },

      handleDrop(lane,dropResult) {
         console.log(dropResult)
         const {removedIndex,addedIndex}=dropResult
         console.log(lane,removedIndex,addedIndex)
         if(lane===this.draggingCard.lane && removedIndex===addedIndex){
           return;
         }
        if(removedIndex!==null){
          this.cards[lane].splice(removedIndex,1)
        }
        if(addedIndex!==null){
          this.cards[lane].splice(addedIndex,0,this.draggingCard.cardData)
        }
      },
      getChildPayload(index){
        return index
      }
    }
}
</script>

<style>
#app {
 
}
.h2.lane-title{
  color: #6D6E6E;
}
.board{
  display: flex;
  justify-content: flex-start;
  margin: 1.2rem 0.8rem;
  align-items: flex-start;
}
.lane {
  background: #D2F3F2;
  width: 30rem;
  
  border-radius: 0.8rem;
  box-shadow: 0 .1rem .2rem 0 rgba(88, 60, 60, 0.1);
  margin: 0 0.8rem;
  padding: 0 0.9rem;;
}
.lane-title {
  margin-bottom: 0.6rem;
  padding: 0.8rem 0.5rem;
}
.placeholder {
  background: rgba(33, 33, 33, 0.08);
  border-radius: 0.08rem;
  transform: scaleY(0.85);
  transform-origin: 0% 0%;
}
.label {
  color: #838383;
}

.kanban-action {
  display: flex;
  justify-content: space-between;
}

.kanban-button {
  max-width: 130px;
  pointer-events: none;
  border: none;
  color: #f7f7f7;
  padding: 1px 20px;
  text-align: center;
  text-decoration: none;
  border-radius: 100px;
}

.kanban-button.todo {
  background-color: #f3c1c1;
}
.kanban-cards {
  display: flex;
  justify-content: center;
  margin: 2rem;
}
.item.todo {
  border-left: 5px solid #f3c1c1;
}
.item .card small {
  color: #838383;
}
.item,
.draggable-item,
.draggable-item-horizontal {
  position: relative;
  display: flex;
  flex-direction: column;
  padding: 10px;
  margin: 8px;
  cursor: pointer;
  user-select: none;
  background-color: white;
  border: 1px solid transparent;
  border-left: 5px solid #e0e0e0;
  border-radius: 4px;
  transition: border-color 0.2s linear;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1), 0 0 1px rgba(0, 0, 0, 0.1);
}
.item .card {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.item.inprogress {
  border-left: 5px solid #c4ebaf;
}
.kanban-button.inprogress {
  background-color: #c4ebaf;
}
.item.done {
  border-left: 5px solid #b3d5e9;
}
.kanban-button.done {
  background-color: #b3d5e9;
}
</style>
