<template>
  <div class="hello2">
    <h5>{{ 'Zone' }}</h5>
    <testa totesta="新建分支b testa 555"></testa>
    <div>-----------------------------------------</div>
    <h3>OBS BrowserJS SDK demo</h3>
    <h4>Basic Configuration</h4>
    <label>AK:</label>
    <input type="text" value="ON9HXKWSMNXSZYI8SJCC" id="ak" size="10"/>
    <label>SK:</label> <input type="text" value="E8K80DPCcY3DCN0XBeFDoSVDxZLEJ4cVWfyZKIIb" id="sk" size="10"/>
    <br>
    <label>Server:</label> <input type="text" value="obs.cn-southwest-2.myhuaweicloud.com" id="server" size="10"/>
    <label>Bucket:</label> <input type="text" value="threecloud.platform" id="bucketname"/>
    <form method="post" enctype="multipart/form-data" id="postForm">
      Object key
      <!-- Object name -->
      <input type="text" name="key" value="objectkey" v-model="fileName"/>
      <br>
      ACL
      <!-- Object ACL -->
      <input type="text" name="x-obs-acl" value="public-read" id="x-obs-acl"/>
      <br>
      Content-Type
      <!-- Object MIME type -->
      <input type="text" name="content-type" value="text/plain" id="content-type"/>
      <br>
      <!-- Customized metadata -->
      <input type="text" name="x-obs-meta-property1" value="property-value1" id="x-obs-meta-property1"/>
      <br>
      <input type="text" name="x-obs-meta-property2" value="property-value2" id="x-obs-meta-property2"/>
      <!-- Base64 code of the policy -->
      <input type="hidden" name="policy" value="*** Provide your policy ***" id="policy"/>
      <!-- Signature information -->
      <input type="hidden" name="signature" value="*** Provide your signature ***" id="signature"/>
      <!-- AK -->
      <input type="hidden" name="accessKeyId" value="*** Provide your Access Key ***" id="accessKeyId"/>
      <br>
      <input name="file" type="file" id="data" autocomplete="off" @change="setObjectName"/>
      <input name="Click to upload file" value="Upload" type="button" @click="doUpload();" />
    </form>
    <div>-----------------------------------------</div>
    <input type="file" id="zpy_input">
    <input name="Click to upload file" value="上传" type="button" @click="doUpload2();" />
    <div>-----------------------------------------</div>
    <form method="post" enctype="multipart/form-data" id="postForm2">
      <input name="file" type="file" id="data2" autocomplete="off"/>
      <input name="Click to upload file" value="Upload" type="button" @click="doUpload2();" />
    </form>
  </div>
</template>

<script>
import testa from '../subcomponents/testa'
import ObsClient from 'esdk-obs-browserjs'

export default {
  name: 'Zone',
  components: {
    testa
  },
  props: {
    mse: String
  },
  data () {
    return {
      fileName: ''
    }
  },
  mounted () {
  },
  methods: {
    getObsClient () {
      var ak = document.getElementById('ak').value
      var sk = document.getElementById('sk').value
      var server = document.getElementById('server').value
      return new ObsClient({
        access_key_id: ak,
        secret_access_key: sk,
        server: server,
        signature: 'obs',
        timeout: 60 * 5
      })
    },
    getObsClient2 () {
      var obsClient = new ObsClient({
        access_key_id: 'ON9HXKWSMNXSZYI8SJCC',
        secret_access_key: 'E8K80DPCcY3DCN0XBeFDoSVDxZLEJ4cVWfyZKIIb',
        server: 'obs.cn-southwest-2.myhuaweicloud.com'
      })
      console.log('obsClient', obsClient)
      return obsClient
    },
    setObjectName () {
      var obj = document.getElementById('data').value
      this.fileName = obj
    },
    doUpload () {
      var obs = this.getObsClient()
      var server = document.getElementById('server').value
      var bucketName = document.getElementById('bucketname').value
      var acl = document.getElementById('x-obs-acl')
      var contentType = document.getElementById('content-type')
      var meta1 = document.getElementById('x-obs-meta-property1')
      var meta2 = document.getElementById('x-obs-meta-property2')
      var formParams = { 'x-obs-acl': acl.value, 'content-type': contentType.value, 'x-obs-meta-property1': meta1.value, 'x-obs-meta-property2': meta2.value }
      var res = obs.createPostSignatureSync({ Expires: 3600, FormParams: formParams })
      var policy = document.getElementById('policy')
      var signature = document.getElementById('signature')
      policy.value = res.Policy
      signature.value = res.Signature
      var accessKeyId = document.getElementById('accessKeyId')
      accessKeyId.value = document.getElementById('ak').value
      var postForm = document.getElementById('postForm')
      postForm.action = 'http://' + bucketName + '.' + server + '/'
      postForm.submit()
      console.log('Creating object in post way')
    },
    doUpload2 () {
      var obsClient = this.getObsClient2()
      var sourceFile = document.getElementById('zpy_input').files[0]
      var postForm = document.getElementById('postForm2')
      console.log('sourceFile', sourceFile)
      console.log('postForm', postForm)
      obsClient.putObject({
        Bucket: 'threecloud.platform',
        Key: 'str.png',
        // SourceFile: sourceFile
        Body: postForm
      }, (err, result) => {
        if (err) {
          console.error('Error-->' + err)
        } else {
          console.log('Status-->' + result.CommonMsg.Status)
        }
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
