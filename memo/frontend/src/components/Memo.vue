<template>
    <div class="memo">
        <div class="act"><button class="btn btn-primary" @click="add()">+ 추가</button></div>
        <ul>
            <li v-for="d in state.data" :key="d.id" @click="edit(d.id)">{{ d.content }}</li>
        </ul>
    </div>
</template>

<script>
import { reactive } from "vue";
import axios from "axios";

export default {
    setup() {
        const state = reactive({
            data: []
        })
        
        const add = () => {
            const content = prompt("내용을 입력해주세요.");
            axios.post("/api/memos", { content }).then((res) => {
                state.data = res.data;
            })
        }

        const edit = (id) => {
            const content = prompt("내용을 입력해주세요", state.data.find(d => d.id === id).content)

            axios.put('/api/memos/' + id, { content }).then((res) => {
                state.data = res.data;
            })
        }

        axios.get("/api/memos").then((res) => {
            state.data = res.data;
        });

        return { state, add, edit };
    },
}
</script>

<style lang="scss" scoped>
.memo {
    .act {
        padding:10px 0 5px 0;
        text-align: right;
    }
    ul {
        list-style: none;
        padding: 0;
        margin: 0;

        li {
            padding: 15px;
            margin: 5px;
            border: 1px solid #eee;
        }
    }
}
</style>