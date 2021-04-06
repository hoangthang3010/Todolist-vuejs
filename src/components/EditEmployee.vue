<template>
    <form>
        <div class="form-group">
            <label for="nameInput">Tên</label>
            <input type="text" class="form-control" id="nameInput" v-model="member.name">
        </div>
        <div class="form-group">
            <label for="addressInput">Địa chỉ</label>
            <input type="text" class="form-control" id="addressInput" v-model="member.address">
        </div>
        <div class="form-group">
            <label for="phoneInput">Số điện thoại</label>
            <input type="text" class="form-control" id="phoneInput" v-model="member.phone_number">
        </div>
        <div class="btn btn-primary" @click="clickSave()">Lưu</div>
    </form>
</template>
<script>
export default {
    name: 'EditEmployee',
    props: {
        member: Object
    },
    methods:{
        clickSave(){
            this.$emit('save', this.member)
        }
    }
}
</script>