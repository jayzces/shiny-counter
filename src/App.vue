<template>
    <div class="app">
        <button class="reset-btn" @click="reset()">Reset</button>

        <div class="container">
            <div class="count">
                <div class="editing" v-if="editing">
                    <input type="number" min="0" v-model="count" />
                    <input type="text"
                        placeholder="Pokemon"
                        v-model="display" />
                    <button class="btn" @click="save()">Save</button>
                </div>
                <div class="display" v-else>
                    <h1>{{ count }}
                        <button class="edit-btn" title="Edit">
                            <EditIcon @click="editing = true"  />
                        </button>
                    </h1>
                    <p>{{ display }}</p>
                </div>
            </div>

            <div class="buttons" v-if="!editing">
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
                count: 0,
                display: 'Encounters',
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
                localStorageKey: 'shiny-counter-hunts',
                editing: false
            }
        },
        methods: {
            add() {
                this.count++
                this.saveToLocalStorage()
            },
            deduct() {
                if (this.count > 0) this.count--
                this.saveToLocalStorage()
            },
            ifKeyExists(arr, key) {
                let check = false
                arr.forEach(pair => pair.key === key ? check = true : null)
                return check
            },
            reset() {
                this.count = 0;
                this.display = 'Encounters';
                this.saveToLocalStorage();
            },
            save() {
                this.editing = false
                this.saveToLocalStorage()
            },
            saveToLocalStorage() {
                localStorage.setItem(this.localStorageKey, JSON.stringify({
                    count: this.count,
                    display: this.display
                }))
            }
        },
        mounted() {
            // get hunts from local storage
            let bytes = localStorage.getItem(this.localStorageKey)
            if (!!bytes) {
                let hunt = JSON.parse(bytes)
                this.count = hunt.count
                this.display = hunt.display
            }

            document.addEventListener('keydown', e => {
                let key = e.key
                if (!this.editing) {
                    if (this.ifKeyExists(this.addKeys, key))
                        this.add()
                    else if (this.ifKeyExists(this.deductKeys, key))
                        this.deduct()
                }
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

    .reset-btn {
        background-color: transparent;
        position: fixed;
        top: 0.5rem;
        right: 0.5rem;
        font-size: 0.75rem
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

    [type="text"],
    [type="number"] {
        display: block;
        padding: 0.5rem 1rem;
        text-align: center;
        width: calc(100% - 2rem);
    }

    [type="number"] {
        font-size: 3.5rem;
    }

    .editing input:not(:first-child) {
        margin-top: 4px;
    }

    .editing .btn {
        margin-top: 1rem;
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
    }

    .deduct .btn {
        background-color: var(--red-light);
        color: var(--red);
    }

    .add button {
        background-color: var(--green-light);
        color: var(--green);
    }

    .tags {
        display: flex;
        justify-content: center;
        margin-top: 0.5rem;
    }

    .tag {
        background-color: var(--blue-light);
        display: inline-block;
        margin: 2px;
        padding: 0.25rem 0.5rem;
        font-size: 0.75rem;
        border-radius: 4px;
    }
</style>
