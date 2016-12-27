<template>
    <div id="app">
        <h1 v-text="title"></h1>
        <!-- <div class="input-group"> -->
            <input id="input-bar" type="text" class="form-control" placeholder="New Todo..." v-model="newItem" v-on:keyup.enter="addNewTodo">
            <!-- <span class="input-group-addon">Add</span> -->
        <!-- </div> -->

        <ul>
            <li v-for="item in items">
                <input type="checkbox" v-model="item.isFinished" v-on:click="toggleFinish(item)">
                <span v-bind:class="{finished: item.isFinished}">{{ item.label }}</span>
                <span class="btn btn-success btn-xs" disabled="disabled" v-show="item.isFinished">Finished</span>
                <button type="button" class="btn btn-warning btn-xs" v-show="item.isFinished" v-on:click="deleteTodo(item)">Delete</button>
            </li>

        </ul>
    </div>
</template>

<script>
import Store from './store'

export default {
    name: 'app',
    data: function () {
        return {
            title: 'Yet Another ToDo-List',
            items: Store.fetch(),
            newItem: '',
            childWords: 'hehe'
        }
    },
    methods: {
        toggleFinish: function (item) {
            item.isFinished = !item.isFinished
        },
        addNewTodo: function () {
            this.items.push({
                label: this.newItem,
                isFinished: false
            })
            this.newItem = ''
            Store.save(this.items)
        },
        deleteTodo: function (item) {
            for (let i = 0; i < this.items.length; i++) {
                if (this.items[i] == item) {
                    this.items.splice(i, 1)
                    break
                }
            }
        }
    },
    watch: {
        items: {
            handler: function (items) {
                Store.save(items)
            },
            deep: true
        }
    },
}
</script>

<style>
#input-bar {
    width: 95%;
    margin: auto auto 20px 30px;
}
.btn.btn-success.btn-xs {
    margin-left: 10px;
}
li {
    text-align: left;
    line-height: 40px;
    font-size: 14pt;
    border-bottom: dotted 1px #8c8c8c;
}
.finished {
    text-decoration: line-through;
    color: #8c8c8c;
}
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
