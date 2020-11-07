<template>
    <div class="app">
        <div class="container">
            <div class="count">
                <h1>{{ counter }}</h1>
                <p>Encounters</p>
            </div>

            <div class="buttons">
                <div class="deduct">
                    <button @click="deduct()">Deduct</button>
                    <div class="tags">
                        <span class="tag"
                            v-for="tag in deductKeys"
                            :key="tag.key">{{ tag.display || tag.key }}</span>
                    </div>
                </div>
                <div class="add">
                    <button @click="add()">Add</button>
                    <div class="tags">
                        <span class="tag"
                            v-for="tag in addKeys"
                            :key="tag.key">{{ tag.display || tag.key }}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                counter: 0,
                addKeys: [
                    {
                        key: '+',
                        display: null
                    }, {
                        key: ' ',
                        display: '<Space>'
                    }, {
                        key: 'ArrowUp',
                        display: '↑'
                    }
                ],
                deductKeys: [
                    {
                        key: '-',
                        display: null
                    }, {
                        key: 'Backspace',
                        display: 'Backspace'
                    }, {
                        key: 'ArrowDown',
                        display: '↓'
                    }
                ]
            }
        },
        methods: {
            add() {
                this.counter++
            },
            deduct() {
                if (this.counter > 0) this.counter--
            },
            ifKeyExists(arr, key) {
                let check = false
                arr.forEach(pair => pair.key === key ? check = true : null)
                return check
            }
        },
        mounted() {
            document.addEventListener('keyup', e => {
                let key = e.key
                if (this.ifKeyExists(this.addKeys, key)) this.add()
                else if (this.ifKeyExists(this.deductKeys, key)) this.deduct()
            })
        }
    }
</script>

<style>
    @import "styles.css";
</style>

<style scoped>
    .app {
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
    }

    .container {
        padding: 1.5rem;
        max-width: 480px;
        width: 100vw;
        text-align: center;
    }

    h1,
    p {
        margin: 0;
    }

    h1 {
        font-size: 3.5rem;
    }

    .buttons {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: 4px;
        margin-top: 1.5rem;
    }

    button {
        display: block;
        padding: 0.75rem 1.25rem;
        width: 100%;
        border: 0;
        border-radius: 4px;
    }

    .deduct button {
        background-color: rgba(220, 20, 60, 0.1);
        color: rgb(220, 20, 60);
    }

    .add button {
        background-color: rgba(39, 194, 76, 0.1);
        color: rgb(39, 194, 76);
    }

    .tags {
        display: flex;
        justify-content: center;
        margin-top: 0.5rem;
    }

    .tag {
        background-color: rgba(0, 170, 255, 0.1);
        display: inline-block;
        margin: 2px;
        padding: 0.25rem 0.5rem;
        font-size: 0.75rem;
        border-radius: 4px;
    }
</style>
