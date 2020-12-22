<template>
    <section>
        <div class="title">投稿</div>
        <form @submit.prevent="addPlace">
        <b-field label="種類">
            <b-select placeholder="Select a type" v-model="type">
                <option value="plant">植物</option>
                <option value="animal">動物</option>
                <option value="disaster">災害</option>
            </b-select>
        </b-field>
        <b-field label="画像">
            <b-upload v-model="dropFiles"
                multiple
                drag-drop>
                <section class="section">
                    <div class="content has-text-centered">
                        <p>
                            <b-icon
                                icon="upload"
                                size="is-large">
                            </b-icon>
                        </p>
                        <p>Drop your files here or click to upload</p>
                    </div>
                </section>
            </b-upload>
        </b-field>
        <b-field label="緯度">
            <b-input v-model="latitude"></b-input>
        </b-field>
        <b-field label="経度">
            <b-input v-model="longitude"></b-input>
        </b-field>
        <div class="has-text-centered buttons" style="margin-top:20px">
            <b-button native-type="submit" type="is-primary" expanded>
                登録
            </b-button>
        </div>
        
    </form>
    </section>
    
</template>

<script>
export default {
    data() {
        return {
            type: null,
            img: null,
            latitude: null,
            longitude: null
        }
    },
    methods: {
        async addPlace () {
            try {
                const back = await this.$axios.$post(`/spot/create`, {
                    type: this.type,
                    img: this.img,
                    latitude: this.latitude,
                    longitude: this.longitude
                })
            } catch (err) {
                console.log(err);
            }
        },
    },
}
</script>

<style>

</style>