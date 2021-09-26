<template>
    <div class="container">

        <div v-if="success" class="alert alert-success">Message sent</div>

        <form @submit.prevent="sendForm">

            <div class="mb-3">
                <label for="name" class="form-label">Name</label>
                <input v-model="name" type="text" class="form-control" name="name" id="name">
                <p v-for="(error, index) in errors.message" :key="index">{{error}}</p>
            </div>

            <div class="mb-3">
                <label for="email" class="form-label" aria-describedby="emailHelp">Email Address</label>
                <input v-model="email" type="email" class="form-control" id="email" name="email" aria-describedby="emailHelp">
            </div>

            <div class="mb-3">
                <label for="subject" class="form-label">Subject</label>
                <input v-model="subject" type="text" class="form-control" id="subject" name="subject">
            </div>

            <div class="mb-3">
                <textarea v-model="message" name="message" id="message" cols="30" rows="10" class="form-control" placeholder="Write your message here"></textarea>
            </div>

            <div class="d-flex justify-content-end mb-3">
                <button type="submit" class="btn btn-dark">{{sending ? 'Sending' : 'Send'}}</button>
            </div>

        </form>
    </div>
</template>

<script>
export default {
    name: 'Contact',
    data() {
        return {
            name: '',
            email: '',
            subject: '',
            message: '',
            errors: {},
            success: false,
            sending: false
        }
    },
    methods: {
        sendForm() {
            this.sending = true;
            this.success = false;
            axios.post('/api/contact-us', {
                'name': this.name,
                'email': this.email,
                'subject': this.subject,
                'message': this.message
            })
            .then(response => {
                if(!response.data.success){
                    this.errors = response.data.errors;
                }
                this.success = true;
                this.sending = false;
                this.name = "",
                this.email = "",
                this.subject = "",
                this.message = ""
            })
        }
    }
}
</script>

<style lang="scss" scoped>

</style>