<template>
	<div class="task_div">
		<input class="input_task" type="text" name="task" v-model="new_task"/>
		<button @click=add_task class="btn_task">Add</button>
	</div>

	<div class="boards">

		<div class=board>
			<p class="title" >To do</p>
			<div  class=drop-zone @drop="onDrop($event, 1)" @dragenter.prevent, @dragover.prevent>
				<div 
					v-for="item in getList(1)" 
					:key="item.id" 
					class="drag-el" 
					draggable="true"
					@dragstart="startDrag($event, item)"
					v-on:dblclick="removeTask(item.id)"
				> 
					{{item.title}}
				</div>
			</div>
		</div>

		<div class=board>
			<p class="title">In Progress</p>
			
			<div class=drop-zone @drop="onDrop($event, 2)" @dragenter.prevent, @dragover.prevent>
				<div 
					v-for="item in getList(2)" 
					:key="item.id" 
					class="drag-el"
					draggable="true"
					@dragstart="startDrag($event, item)"
					v-on:dblclick="removeTask(item.id)"
					> 
					{{item.title}}
				</div>
			</div>
		</div>
		
		<div class=board>
			<p class="title">Done</p>
			<div class=drop-zone @drop="onDrop($event, 3)" @dragenter.prevent, @dragover.prevent>
				<div 
					v-for="item in getList(3)" 
					:key="item.id" 
					class="drag-el"
					draggable="true"
					@dragstart="startDrag($event, item)"
					v-on:dblclick="removeTask(item.id)"
					> 
					{{item.title}}
				</div>
			</div>
		</div>
	</div>
	
</template>

<script>
	import {ref} from "vue"

	export default {
		data() {
			return {
				items: [
					{id: 0, title: "Item A", list: 1},
					{id: 1, title: "Item B", list: 1},
					{id: 2, title: "Item C", list: 2}
				]
			}
		},
		methods:{
			add_task(){
				const task = this.new_task
				console.log(task)
				if(task != undefined){
					const last_id = this.items[this.items.length - 1].id
					this.items.push({id:last_id+1, title:task, list:1})
					this.new_task = ""
				}
				
				
			},

			removeTask(id){
				this.items.splice(this.items.findIndex(e => e.id === id),1);
			},

			
			getList(list){
				return this.items.filter((item) => item.list == list)
			},
	
			startDrag(event, item){
				event.dataTransfer.dropEffect = 'move'
				event.dataTransfer.effectAllowed = 'move'
				event.dataTransfer.setData('itemID',item.id)
			},
	
			onDrop(event, list){
				const itemID = event.dataTransfer.getData('itemID')
				const item = this.items.find((item) => item.id == itemID)
				item.list = list
			}
		}
	}
</script>

<style>

	.boards{
		display: flex;
		padding: 15px;
	}
	
	.task_div{
		margin-top:15px;
		margin-left:35%;
	}

	
	.btn_task{
		margin-left:5px;
		width:20%;
		height:25px;
	}
	
	
	.input_task{
		width:45%;
		height:15px;
		padding:7px;

	}

	.text{
		margin-left:45px;
	}

	.board{
		margin-right:10px;
		
		min-height: 10px;
		width: 50%;
		
	}
	
	.drop-zone{
		text-align: center; 
		background-color: #ecf0f1;
		padding: 10px;
		
		border-radius:15px;
	}	

	.drag-el{
		background-color: #3498db;
		color: white;
		padding: 5px;
		margin-bottom: 10px;
	}
</style>







