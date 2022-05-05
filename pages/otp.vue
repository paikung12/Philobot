<template>
<div>
    sdsd
   <center class="p-4"> <img :src="qr" alt=""><br><br>
      <v-text-field outlined v-model="key" name="name" label="label" id="id"></v-text-field><br><br>
    <v-btn @click="test()" color="success">text</v-btn>
   
   </center>
 
</div>
</template>

<script>
import qrcode from 'qrcode';
import {
    authenticator
} from '@otplib/preset-default';
const user = 'Authen Login';
const service = 'Philobot';

export default {
    data: ({
        dd: 'ddddddddddd',
        qr: '',
        token: '',
        secret:'',
        key:'',
    }),
    async created() {
        const secret = authenticator.generateSecret(); // base32 encoded hex secret key
        const token = authenticator.generate(secret);
        const otpauth = authenticator.keyuri(user, service, secret);
        this.secret = secret
        qrcode.toDataURL(otpauth, (err, imageUrl) => {
            if (err) {
                console.log('Error with QR');
                return;
            }
            if (imageUrl) {
                this.qr = imageUrl
                this.token = token
                   this.secret = secret
            }
        });
    },
    methods: {
        async test() { 
            console.log(this.key, this.secret)
            try {
                let isValid = authenticator.check(this.key, this.secret);  
                if(isValid){
                    alert("OK SUCCESS")
                }else{
                     alert("ERROR KEY")
                }
            } catch (err) { 
                console.error(err);
            }
        }
    }
}
</script>

<style>

</style>
