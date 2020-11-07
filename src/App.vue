<template>
    <div class="app">
        <div class="container">
            <div class="count">
                <h1>{{ hunt.count }}
                    <button class="edit-btn" title="Edit">
                        <EditIcon />
                    </button>
                </h1>
                <p>{{ hunt.display }}</p>
            </div>

            <div class="buttons">
                <div class="deduct">
                    <button class="btn" @click="deduct()">Deduct</button>
                    <div class="tags">
                        <span class="tag"
                            v-for="tag in deductKeys"
                            :key="tag.key">{{ tag.display || tag.key }}</span>
                    </div>
                </div>
                <div class="add">
                    <button class="btn" @click="add()">Add</button>
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
        components: {
            EditIcon: () => import('./svg/edit.svg')
        },
        data() {
            return {
                hunt: {
                    count: 0,
                    display: 'Encounters'
                },
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
                ],
                localStorageKey: 'shiny-counter-hunts'
            }
        },
        methods: {
            add() {
                this.hunt.count++
                localStorage
                    .setItem(this.localStorageKey, JSON.stringify(this.hunt))
            },
            deduct() {
                if (this.hunt.count > 0) this.hunt.count--
                localStorage
                    .setItem(this.localStorageKey, JSON.stringify(this.hunt))
            },
            ifKeyExists(arr, key) {
                let check = false
                arr.forEach(pair => pair.key === key ? check = true : null)
                return check
            }
        },
        mounted() {
            // get hunts from local storage
            let bytes = localStorage.getItem(this.localStorageKey)
            if (!!bytes) {
                this.hunt = JSON.parse(bytes)
            }

            document.addEventListener('keydown', e => {
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

    h1 {
        margin: 0 auto;
        position: relative;
        font-size: 3.5rem;
        width: max-content;
        max-width: 100%;
    }

    p {
        margin: 0;
    }

    .edit-btn {
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        top: 0;
        right: -2.5rem;
        width: 28px;
        height: 28px;
        font-size: 1rem;
        border-radius: 50%;
        cursor: pointer;
        z-index: 1;
        opacity: 0;
    }

    .count:hover .edit-btn {
        opacity: 1;
    }

    .edit-btn svg {
        width: 16px;
        height: 16px;
    }

    .buttons {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: 4px;
        margin-top: 1.5rem;
    }

    .btn {
        display: block;
        padding: 0.75rem 1.25rem;
        width: 100%;
        cursor: pointer;
    }

    .deduct .btn {
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
