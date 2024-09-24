# Crytography/Bit by Bit

The idea of this challenge was to break an improperly encrypted one time pad encryption.

To start, we were given 2 files. 
- A python file named `transmit.py`
- A text file named `out.txt`

We open transmit.py and this is its contents:

    #!/usr/bin/python3
    import sys

    blocksize = 16

    def loadMessage():
        message = ""
        with open("message.txt","r",encoding='utf-8') as file:
            for line in file:
                message += line
        while len(message) % blocksize != 0:
            message += '0'
        return message

    def encode(chunk):
        start = 120
        encoded = 0
        for c in chunk:
            encoded = encoded | (ord(c)<<start)
            start -= 8
        return encoded

    def transmit():

        # Test key
        key = 0xadbeefdeadbeefdeadbeef00

        iv = 0
        msg = loadMessage()
        chunks = [msg[i:i+16] for i in range(0,len(msg), 16)]

        send = ''
        for chunk in chunks:
            iv = (iv+1) % 255
            curr_k = key+iv
            encoded = encode(chunk)
            enc = encoded ^ curr_k
            foo = hex(enc)[2:]
            while len(foo) != 32:
            foo = '0'+foo
            send += foo
        print(send)
        sys.exit(0)

    if __name__=="__main__":
        transmit()


I also take a look at out.txt and these are the contents:

    43686170daa83933a16577820c9abb6f69676d61daa42833d32a478f63f3902261206469c3a13233fc2c57ca1bd5916e20696e20caa23c7de42a548449f9966d6361676f82ed0e67f8244dca4bf99775686572228e9f2e6afe2a4f8e1a9a8d6774206875c0ae2376f4654c9c0cc8de6f69732063c1a03b66e42051c649ce966d20676c6fd9ed2d61ff28039e01dfde646f6e6974c1bf6b70f136578307ddde6b20626c75c7be2333f83046ca08d98c6473732068c7be6b77f531469804d3906964206661cda86533c42d46ca0ad6916e6b206f6e8eb92376b0324286059a8a67636b65648ebd2a60e4654e830dd4976868742c20ccb83f33d5314b8b079a8e716964206ec1ed267afe210dca21dfde6661732064cba83b33f92b578549d98c73636b696ec9ed247df5654c8c49ce9676206d6f73daed227de4374a8908ce9b34656e6372d7bd3f7aff2b039910c98a706d732068cba96b76e62051ca0cd49d79756e7465dca82f33f165408208d692726e676520dea23867f5210385079a9f76206f6273cdb83976b0234c981cd7de726e6f776e8eab2461b02c579949d98c6370746f67dcac3b7bf926039a1cc0847765732e20faa52e33f629428d49d2977864656e20d9a43f7bf92b0fca19d98a7b7b345f74c6fe1467e7757c9e58d7cd41346133329af87a23a370159763b0aa7765206368cfa12776fe2246ca1edb8d0073696d70c2a86b7afe65408507d99b5174206275daed2f72e52b578307ddde4b6e206578cbae3e67f92a4dca0ddf9d4c646520618ebe2e61f92050ca06dcde416e637279deb92e77b02846991adb994073207769daa5227db024039d0cdf9508205468658ebf2e64f13747ca1edb8d0774686520c5a42577b02a45ca07d58a4772696574d7ed3f7bf131038906cf924d206d616bcbed2461b027518f08d1de4b20686163c5a83960b037469a1cce9f5f696f6e20c7a36b67f820039f07de9b5e67726f75c0a96b70ff284e9f07d38a542e20466fdced0e67f8244dc649d38a0e77617320c3a23976b0314b8b079a945a73742066cfa02e28b02c57ca1edb8d1061206361c2a1227df76b29e02ad29f41746572209ced1f7bf56565831bc98a12456e636fdba33f76e24f77821bdf9b13646179738ea42567ff6557820c9a9d5c616c6c65c0aa2e3fb000578208d4de5d697420618eba2a7ffc6b03a4069a9357747465728ea52464b0284284109a9f5b676f7269daa52660b02d46ca1dc8975d642c2074c6a86b70ff2146ca1ed58b55646e7420ccb82f74f56b03ac1bcf8d4e72617465cae16b7bf565478f0ad39a5e6420746f8eb92a78f56542ca0bc89b5d6b20616ecaed3870f12b039e01dfde5b6f72756d8eab2461b0244d9349d2975074732e20efbe6b7bf565419806cd8d5a642c20618ebd397ae624578f49d79b33736167658ea32467f9234a8908ce972e6e20706fdebd2e77b03053c463b0dc0c65656420c6a82763b0324a9e019a8a2b6520656ecdbf3263e42c4c84569ab76463616e20c2a82577b024038208d49a6b202d2044cfb92a43f93d4a8f4bb0f4037468616e8eba2a60b036488f19ce9724616c2062dbb96b70e5374a851cc9d06848656420c6a82a61f4654c8c49fe9f3d61506978c7a86733f1654e931ace9b38696f75738eab2274e53746ca02d4913c6e20666fdced3f7bf52c51ca19c8913b65737320c7a36b77f13142ca08d49f21797369738eac2577b026519319ce912972617068d7e36b41f52956891ddb903b6c792c20c6a86b61f5354f830cded27022537572cbe16b64f82457ca0dd5de286f752068cfbb2e33f92b038700d49a6d220a0a57c7b9237afe654e8307cf8a36732c2044cfb92a43f93d4a8f49c89b27706f6e64cba96b64f9314bca089a8d30726965738ea22d33e331469a1a9a9f3864206120dba32262e520038e0cd98c2e7074696fc0ed3f7cff290dca2cce96396e20666fc2a12464f521039e01dfde306e737472dbae3f7aff2b50c649db903e20746f20c6a43833f12842900cd79b35742c2068cbed2672f420039900dd903566696361c0b96b63e22a44980cc98d7320496d70dca83860f5210fca01dfde376e766974cba96b57f13142ba00c2973a20746f20c4a2227db02d4a8749d5904074686520debf2479f52657c463b0bd0961707465dced7833c42d46ca28d6920b65732041ddbe2e7ef22946e03ed38a0b20446174cf9d226bf9200385079a9c0b6172642c8e883f7bf12b50ca0ad590036964656ecda86b74e22054c449ee96037920636fc3a03e7df926429e0cdede046f6e7374cfa33f7fe969039901db8c016e672074c6a82461f92050ca08d49a49626f756ecda42574b02c478f08c9de0566662065cfae2333ff314b8f1b94de2f61746150c7b52276e365518f08d6de02616d6520d9ac3833dc2c4f8b459a9f4d6272696cc2a42a7de465539806dd8c0f6d6d65728eba2267f86542ca02d49f0c6b20666fdced2d7afe214a840e9a8e1174746572c0be6b64f820518f49d58a1965727320ddac3c33ff2b4f9349d996136f732e0aa48c3833e42d46ca0dcf9153776f726bcba96733e42d469349c89b156c697a65caed3f7bf53c03840cdf9a1064206d6fdca86b76e83546981dd38d132e204c69c2ac6b60e522448f1ace9b1320636f6edaac2867f92b44ca08d4de176c642066dca42e7df469038b49d29f0b64776172cbed3863f5264a8b05d38d0e206e616dcba96b5ef13740854998bc12744e696ec4ac6933d129558b1bdf8452204d6172cda23833f420469a49cf901965727374cfa32f7afe2203850f9a961f72647761dca86671f136468e49df901c72797074c7a22533f32a56860d9a8ef26f766964cbed3f7bf565468e0edfdef568657920c0a82e77f5210de063f79ff0636f2c20c7a33f61f922568f0d9a9cfa207468658eae2372fc2946840edfd2a46a6f696ecba96b64f9314b851ccedeed65736974cfb9227cfe6b03a200c9dee56f6e7472c7af3e67f92a4dca1edb8da7696e7661c2b82a71fc200fca06dc98ed72696e678ea42560f9224b9e1a9a97e7746f2074c6a86b63f83c50830adb92aa6c617965dced2475b0314b8f49df90e872797074c7a22533e42d429e49d49be5746865728e883f7bf12b038406c8dec1696c6120c6ac2f33f32a4d9900de9bfc65642e0aa48e2372e0314698498ededb68652052c1ac2f71fc2a408163fe9be3706974658eb92376f937038906d79cf86e656420cbab2d7ce23150c649ce96f720747269c1ed2e7df32a56841ddf8cf664206120c0a83c33e22a428e0bd691f76b2e2054c6a86b76fe26519319ce97fa6e206861caed2772e920519949cd97e268696e20c2ac3276e2360fca08d49ab765766572d7ed3f7afd20039e01df87b87065656ccba96b7cfe20038808d995b520616e6fdaa52e61b0284c980c9a9df56d706c65d6ed247df565429a19df9fe965642e20faa52e7ae26545981cc98aee6174696fc0ed2c61f5320fca0bcf8abd67697669c0aa6b66e065548b1a9a90f17420616e8ea23b67f92a4dc463b0bbeb68616e20caa8287af42047ca1dd5dec272696e678ea42533ff2b46ca04d58cc420706572dda22533f12b038505dedec16f6c6c65c9a86b75e22c46840d9a9fcd64206d61daa52e7ef1314a8900db908820416e79cfed6949f5374cae08c3dc8550617465c2e36b52fe3c429949c98ec36369616cdab46b64f1360383079a8acf656f7265daa42872fc654e8b1dd29bc561746963dded2a7df465529f08d48adc6d20636fc3bd3e67f92b44c449e996cf206861648eac6b61f535569e08ce97c46e20666fdced3f7bf92b488307dddec375747369caa86b67f820038806c2d28d77686963c6ed3c72e36553980cd997dd656c7920d9a52a67b0314b8f109a90ca6564656480c74152fe3c42ca1edb8d90696e7472c7aa3e76f465419349ce96d420636861c2a12e7df720038b07deded86f696e65caed3f7bf565578f08d7d09348657220c8bf2e60f865538f1bc98ed163746976cbed2961ff3044821d9a90d077206c69c8a86b7afe314cca1dd29bdf72206566c8a23967e36b03b901dfdec475676765ddb92e77b030508307dddec975616e74dba06b72fc224c9800ce96d47320746f8eb92a70fb2946ca1dd29b9a656e6372d7bd3f7aff2b0fca089a8cd2736b7920ccb83f33e02a578f07ce97dd6c6c7920c9bf2466fe2141980cdb95d46e672061debd397cf1264bc463b0bdd661707465dced7e33c42d46ca2bc89bde6b746872c1b82c7b9a124a9e019abfae79617320dfb82a7de4304eca1ddf9da96e697175cbbe6733e42d46ca1ddf9faf206d6164cbed2a33f237468b02ce96b16f75676880ed1f7bf53c038708d49fa365642074c1ed2f76f32a478f49dbdeb669676e69c8a42872fe31039a06c88aaf6f6e206fc8ed3f7bf56546840ac887b774656420c3a83860f12246c649c89bbe65616c69c0aa6b64f82457ca08ca8eac617265648eb92433f220038906d58cae696e6174cbbe6533d82a548f1fdf8ce7207468658eae247ce2214a8408ce9bbf20776572cbed227df32a4e9a05df8aa82c206c65cfbb227df76557820cd7deb969746820cfed3f72fe31428600c097a16720636cdba86b71e5310384069a9dbc65617220caa43976f3314a850794f4db4c696c6182ed2e65f537039e01dfdeb665746563daa43d76bc65509f0edd9ba074656420daa52e6ab02c4d9c0cc98abd676174658eb92376b03542981dd39fb920636f6fdca9227df1314699479aaba5696e6720c1bd2e7dbd364c9f1bd99bf7696e7465c2a12274f52b408f49ce91b76c732c20daa52e6ab031518b0adf9af974686520cda22461f42c4d8b1ddf8dfa746f20618ebf2e7eff3146ca05d59dba74696f6e8ea42533e42d46ca3acd97af7320416cdebe6533d931039d08c9debc6e206f6ccae16b72f2244d8e06d49bba20666163c7a12267e965519f04d58cba6420746f8ea52a65f565418f0cd4de9573656420c8a23933e32040980cce979765207072c1a72e70e436038e1cc8978c67207468cbed087cfc2103bd08c8d0e90a436861deb92e61b07303be01dfdeae6f75726ecbb44157f531469804d390806420746f8eb82570ff33469849ce968320747275daa56733e42d46ca1ddf9f8a20646563c7a92e77b0314cca04df9b9c20696e20dea83960ff2b038c06c8de9d68652066c7bf3867b0314a870c9a9f8464207472cfbb2e7fb0314cca3acd979f7a65726ccfa32f3db0114b8f109a8e836f6c65648eb92376f93703980cc991987263657382ed2a7df46554831dd2978020646179dde16b67f8205aca1edf8c8a206f6e20cfed3b7ff12b46c649df86936974656dcba33f33f12b47ca08ca8e836568656edda4247db03654831bd6979c67207769daa5227db0314b8f0494f4f9496e2074c6a86b52fc3550c649ce9691206a6f75dca32e6ab0314cca1dd29bd56162616ecaa22576f465458b0ad3929f74792077cfbe6b67e220428901df8c9875732e20fda32464bd264c9c0cc89b9c20706174c6be6b72fe2103830ac3de8e696e64738eb92e60e42047ca1dd29b9372207265dda22765f56b03a81ccede8f686569728ea92e67f5374e8307db8a956f6e206ecbbb2e61b032429c0cc89b992e205768cba36b67f8205aca0fd3909f6c6c7920dca82a70f82047ca1dd29b2066616369c2a43f6abc654a9e49cd9f7220612064cbbf2e7ff92657ca0bcf976e64696e678ea52a7ff668419f1bd39b6720696e20dda32464be4f29a307c99760652c2074c6a83233f62a56840d9a91696420636fc3bd3e67f53750ca08d49a26646f6375c3a82567e36b03ab04d59060207468658ebf2e7ff92650ca1edb8d2861206475ddb93233f824518e49de8c6076652e20e3ac3970ff36039902d3926673206361c3a86b7afe314cca19d69f7220617320c6a86b70f137468c1cd6927520657874dcac2867f521039e01dfde696174612e8e992433e42d46831b9a9f7d746f6e69dda52676fe310fca1dd29b2f64726976cbed287cfe31428307df9a3074686520cda22663fc20578f49df907272797074cba96b7ef536508b0edfde6668657920c6ac2f33f220468449ce8c6a696e6720daa26b70e224408147b0f45768617074cbbf6b24b0114b8f49e89b63656c6174c7a22519d224408149db8a3674686569dced2672fb20508200dc8a376261736582ed3f7bf565578f08d7de6f6f726b65caed3f7ae2204f8f1ac9926020746f20caa82861e93557ca1dd29b3a66696e61c2ed2676e336428d0c94de4f68652063c1a0297afe2047ca0cc28e7972746973cbed2475b000578208d4d23d4c696c6182ed0672e2264cc649db907a20416e79cfed2d7afe244f86109a9c7072652066dcb82267be6577820c9a934573736167cbed3976e62042860cdede4020636f6eddbd2261f1265aca1dd29f5620737061c0a32e77b021468908de9b502c20696ed8a22765f92b44ca0ad69f4a64657374c7a32e33f72a558f1bd493406e74206fdea83972e42c4c841a9a9f4864206869caa92e7db026428901df8d076f662069c0ab2461fd24578306d4d0220a5468658ea6257ce729468e0edfde5d68657920dba3287ce620518f0d9a894b7320626fdaa56b67f8374a8605d3904c20616e648eb92e61e22c459300d4990220497420c7a03b7ff926429e0cdede5d6f776572c8b82733f92b47831fd39a5b616c7320cfa32f33f53d53851adf9a0f73656372cbb93833e42d429e49d29f5420626565c0ed2966e22c468e49dc914320796561dcbe6533c42d469349d1905777207468cbb46b7bf121039e069a8a4165616420cdac3976f6304f861094f43e43686170daa83933a86577820c9aba5063697369c1a34144f9314bca1dd29b1664656372d7bd3f76f4654e8f1ac99f506520696e8ea52a7df469039e01dfde4c65616d20c8ac2876f46542ca04d58c586c206469c2a8267ef16b03b80ccc9b5b6c696e678eb92376b02c4d8c06c8935a74696f6e8eae2466fc21038208cc9b1c6661722ddca82a70f82c4d8d49d9915373657175cba32876e369038806ce961e676f6f648eac2577b027428e479aaa5765792064cbaf2a67f521038606d49960696e746f8eb92376b02b4a8d01ced26177656967c6a42574b0314b8f49ca9136656e7469cfa16b75ff3703801cc98a2a63652061c9ac227de331039e01dfde2768616f738ea43f33f32a56860d9a8b2b6c656173c6e34119d92b039e01dfde236e642c20daa52e6ab021468900de9b2320746f20c2a82a78b0314b8f49d3902e6f726d61daa4247db0314cca1dc88b3a74656420c4a23e61fe244f831ace8d6a6b6e6f77c0ed2d7ce26557820cd38c6b696e7465c9bf2267e96b03be01dfde3f746f72798eaf397cfb200fca0adb8b3e696e6720cfed2676f42c42ca0fc89b207a792e20f9a5227ff56557820c9a8920726c6420c9bf2a63e029468e49cd972468207468cbed3976e6204f8b1dd3913f732c2074c6a86b67f5244eca18cf9737746c7920d9a82567b0314b8f00c8de2065706172cfb92e33e7245a99459a953a6f77696ec9ed3f7bf53c038208dede3861646520cfed2f7af62346980cd49d332e0a0a45dea4277cf73046ca3dd29b774c656761cdb44156e42d428449c89b2c75726e65caed3f7cb0064b830adb99362c204c69c2ac6b67ff654b8f1b9a9a3b7461206ccfaf383fb00842980ad5de2f6f206869dded2372e221548b1bdfde2a656e7475dca8383fb0244d8e49fb90246120746f8ea52e61b034568b07ce8b3320726573cbac3970f86b03be01df877f73746179cba96b7afe6557851cd9964c20626f75c0a96b71e96557820c9a9f0576656e74dbbf2e33e42d469349d29f0620736861dca82f3db0114b8f49d996026c6c656ec9a86b7bf12103881bd58b0368742074c6a82633e42a448f1dd29b172c20666fdcaa227df765418507de8d46746861748eba2466fc21038608c98a4761206c69c8a83f7afd200de063ee960d20656e63dcb43b67f52103850dc38d1a65792077cfbe6b7ce62051c649d88b1e207468658eab397af52b479901d38e1820746865d7ed2372f46545851bd79b0820616e648eb92376b036488305d68d4d746865798ea52a77b02d4c840cdede1c656d6169c0a82f3db0124c9d479aaa0769732069dded207afe2103850f9a9f506372617ad7ed3867ff375aca00c990567420697480c77b23a07513da598ace42

My first thought after seeing these is that the plaintext came from `message.txt`, since that file is read from by transmit.py. Immediately I start writing some code that will reverse the functions of transmit.py.

Writing this reversing code helps me understand the encryption algorithm, so I will explain each function of `transmit.py` now.

### Understanding `transmit.py`

`load_message()` opens `message.txt` and then appends all of the lines into one long string. Then, it checks the length of that string. If the length of the string is not evenly divisible by `blocksize` then it will append `0` onto the end of the message until it is divisible by `blocksize`. In our case, `blocksize` is set to 16. The function returns the string.

---

`encode(chunk)` takes a `chunk: str` as a parameter. Whenever it is called, chunk is always of length 16, which is important for how this function works. Then it creates an integer `encoded` and sets it to 0. Then we enter a for each loop. On each iteration of the loop, this code is executed:

    encoded = encoded | (ord(c)<<start)
    start -= 8

This will take a character of chunk, get its ASCII value using `ord()`, then use a bitwise OR between that ASCII value bitshifted left (starting with 120 bits), and `encoded`. It then decreases the amount of bits shifted left by 8 and iterates. It will return the resulting integer `encoded`. If you are unfamiliar with ASCII values, here is some information, along with a table: [ASCII Values](https://www.asciitable.com/). Understanding what happens here is key to understanding this method of encryption.

At the start of the for each loop, `encoded` is 0. The binary representing the ASCII value of the first character in chunk `c` is bitshifted over 120 bits. Then the bitwise OR is done. This effectively sets the 8 most significant bits of `encoded` to the ASCII value of `c`, since bitwise OR between `c` and all 0's will just give the bits comprising `c`. Then, it will repeat this process with the next 15 characters, all the way down until the least significant bits are set to the ASCII value of the last character of `chunk`. 

We can understand that `encoded` is basically the ASCII values of a chunk, back to back. If we could retrieve `encoded`, we could go over each byte, decode them into an ASCII value, and then we could assemble those back into the plaintext comprising `chunk`.

---

`transmit()` first sets a key for encryption. Our ultimate goal is going to be to find what this key is, but first let's understand the rest of the function. It retrieves the plaintext message using `load_message()` and then splits the string into a list containing groups of 16 characters using this list comprehension: `chunks = [msg[i:i+16] for i in range(0,len(msg), 16)]`. It sets a counter `iv = 0`. Then it will iterate through each chunk of the message. First, it increments iv: `iv = (iv + 1) % 255` Then, it sets a new key `curr_key = key + iv` It will use `encode()` to encode the chunk. Then it executes this. `enc = encoded ^ curr_k` This uses bitwise XOR to encrypt the encoded message returned by `encoded()` using curr_key. It will take the resulting integer, convert it to hex (base 16), pas it with 0 to make sure it has length 32 (this makes sense because you need 32 hex digits to represent 128 binary digits) then add that to a string. It will do this for every chunk, then print the result to console. 

Finally, we can move on.

### Reversing transmit.py

Everything is relatively easy to reverse. The only that may be hard to understand is how to reverse the bitwise operators. Let's go over the one in `transmit()` first. Remember that the bitwise XOR looks like this: `enc = encoded ^ curr_k`

Bitwise XOR has a truth table that looks like this:
| A | B | XOR |
|----|----|:----:|
|0|0|0|
|0|1|1|
|1|0|1|
|1|1|0|

Basically, an odd number of inputs must be 1 for XOR to be 1. Let column A represent a bit of `encoded` and column B represent a bit of `curr_key`. The XOR column represents the resulting bit of `enc`.
We have the row XOR and we need to retrieve A, which is the decrypted encoded message. If we were to imagine using columns XOR and B as an input, how could we get the column A. It turns out, that it is simply using bitwise XOR again. Using this, we can code a function that reverses `transmit()`, which I am conveniently calling `untransmit()`.

    def untransmit(key: int = 0xadbeefdeadbeefdeadbeef00):

        send = load_enc_message()
        iv = 0
        msg = ''
        for i in range(0, len(send), 32):
            iv = (iv + 1) % 255
            curr_k = key + iv
            enc = int(send[i:i+32], 16)
            encoded = enc ^ curr_k
            chunk = decode_to_str(encoded)
            msg += chunk
        return msg

Now, we have to figure out how to reverse `encode()`. I called my reversing function `decode_to_str()` as you can see in the code above. How is that function written?

---

Our goal is to get each byte of the integer, then convert it to ascii and add those onto a string, as we discussed earlier. Here is my code to do that:

    def decode_to_str(encoded: int):
    chunk = ''
    while encoded != 0:
        chunk = chr(encoded & 255) + chunk
        encoded = encoded >> 8
    return chunk

Here, instead of using bitwise OR with 0, I use bitwise AND with 255. Since 255 is represented by 11111111 in binary, using bitwise AND between that and any other set of 8 bits will get those 8 bits. Then, I convert it to an ascii value using `chr()`, and add it to a chunk. Since I'm doing the lowest 8 bits first, I am technically decoding this backwards, which is why I add the last decoded characters to the front of the result string.

From here, we have the code to reverse the original `transmit.py`. We are just missing the key. This seems problematic, especially since the key could be anything between 0 and $2^{128}$. 

### Finding the Key

However, there is a potential problem that happens when the key that is used to encrypt each integer is not as long as the potential encoded integers. When this happens, there will be some bits in the original message that will not be encrypted. Why? Well, suppose you do `11011 ^ 101`. What happens to the most significant bits of the first term? Well, 0's are appended to the second term and the XOR is done like that. So in reality, we are doing `11011 ^ 00101`. When this happens, the upper bits do not get encrypted; they stay the same in the encrypted result, since XOR with 0 preserves identity. This encryption vulnerability is present in this challenge, and we can see this if we run our current reversal algorithm on `out.txt`.

Some of the current result looks like this:

    ChapwÖíÛ\¡$TnigmawÇí~¨QÎM a dinÝíQ¸¶k~m in gÓ£I»ZäGyicago/Sá¹U¢æGxpher"#!Á´S P·$bat humÌ¨YÛ£B¡v1his clÔ¸I¾äpye glotSÂ¿Rì@¬a1monitl®\¸]ªc1a bluj ÌíU©¥gcoss hj ©X©F©med fa`íi(...)

You can definitely see that there are bits of plain text popping up in this partially decrypted message. Using these, we can begin to infer what the rest of the plaintext looks like. Lets use the above as an example. The start of that string looks like Chapter. Since this is from the beginning of the file, we can certainly infer that this is going to be Chapter 1, so we need to find a key such that the start of the integer representing chunk 1 will decrypt to Chapter 1. 

---

This proves to be a little bit annoying do to by hand, so I wrote some other programs to make this a bit easier. Firstly, I made a new text file, which I called `keyfinding.txt`. Then I wrote a small python script that took 2 numbers that would be written in `keyfinding.txt` and converted them to binary.
    with open('keyfinding.txt') as file:
        data = file.readlines()
    if len(data) < 2:
        raise ValueError

    int1 = int(data[0])
    int2 = int(data[1])
    for i in range(len(data)-1, 1, -1):
        data.pop(i)

    int1_bin = bin(int1)[2:]
    while len(int1_bin) % 8 != 0:
        int1_bin = '0' + int1_bin
    bytes1 = [int1_bin[i:i+8] for i in range(0, len(int1_bin), 8)]

    int2_bin = f"{int2:08b}"
    while len(int2_bin) % 8 != 0:
        int2_bin = '0' + int2_bin
    bytes2 = [int2_bin[i:i+8] for i in range(0, len(int2_bin), 8)]

    # this condition must be true in order for the decryption algorithm to even be possible
    while len(bytes2) < len(bytes1):
        bytes2.insert(0, "00000000")

    ret_bin = ''
    ret_str = ''
    for i in range(0, len(bytes1)):

        dec_byte = (int(bytes1[i], 2) ^ int(bytes2[i], 2))
        ret_bin += f"{dec_byte:08b} "
        ret_str += chr(dec_byte)

    data.append(' '.join(bytes1) + '\n' + ' '.join(bytes2) + '\n')
    data.append(str(ret_bin) + '\n')
    data.append(str(ret_str.encode()))
    with open('keyfinding.txt', 'w') as file:
        file.writelines(data)

Now this code is not optimally written at all, but it does the trick. It will iterate through each integer and convert them to bits written to the same file, and it will also add the resulting XOR operations between those bits to the file as well, as binary and as a string. Now, we just need the integers. Remember, the first integer comes from the file and the second comes from the key. The way I found them was just using debugging in my `untransmit()` function. The first chunks encrypted integer was `89600250925833843173610965042831473519` and the current key was `53771735358105774395121135361`. Now, inputting those into `keyfinding.txt` and running the above script will get this result:

    89600250925833843173610965042831473519
    53771735358105774395121135361
    01000011 01101000 01100001 01110000 11011010 10101000 00111001 00110011 10100001 01100101 01110111 10000010 00001100 10011010 10111011 01101111
    00000000 00000000 00000000 00000000 10101101 10111110 11101111 11011110 10101101 10111110 11101111 11011110 10101101 10111110 11101111 00000001
    01000011 01101000 01100001 01110000 01110111 00010110 11010110 11101101 00001100 11011011 10011000 01011100 10100001 00100100 01010100 01101110 
    b'Chapw\x16\xc3\x96\xc3\xad\x0c\xc3\x9b\xc2\x98\\\xc2\xa1$Tn'

Now, we need to find the binary for the first non-zero byte of the key in order for the 5th byte of the top binary XOR the associated bytes in the key to equal the ascii value for `t`. The ascii value for `t` is 116, which is `01110100`. The associated byte in the encrypted text is `11011010`
In order to get the key needed, we just XOR those 2 bytes, since we know that the inverse of XOR is XOR, like we found earlier. This gives us a new key value of `10101110` for the 5th byte. Now, I don't have a good way of converting what I just found back into an integer, so I just wrote a small script to do it for me.

    with open('keyfinding.txt') as file:
    data = file.readlines()

    if len(data) > 2:
        int1 = int(''.join(data[2].strip().split(' ')), 2)
        int2 = int(''.join(data[3].strip().split(' ')), 2)

        data[0] = str(int1) + '\n'
        data[1] = str(int2) + '\n'

        with open('keyfinding.txt', 'w') as file:
            file.writelines(data)

I went ahead and found the next couple of bytes of the key using the same method and converted all of that binary back to an integer. This was the resulting `keyfinding.txt`:

    89600250925833843173610965042831473519
    54098576040339431089512574721
    01000011 01101000 01100001 01110000 11011010 10101000 00111001 00110011 10100001 01100101 01110111 10000010 00001100 10011010 10111011 01101111
    00000000 00000000 00000000 00000000 10101110 11001101 01001011 00010011 10010000 10111110 11101111 11011110 10101101 10111110 11101111 00000001
    01000011 01101000 01100001 01110000 01110100 01100101 01110010 00100000 00110001 11011011 10011000 01011100 10100001 00100100 01010100 01101110 
    b'Chapter 1\xc3\x9b\xc2\x98\\\xc2\xa1$Tn'

---

Now at this point, we can't do anything else with this chunk, but we have gathered more of the key, which means we should be able to decrypt more of the message. We can use this key in our original `untransmit.py` file and gather more of the message. At this point, we can find another partially decoded chunk that lines up in such a way that we can infer some of it in order to retrieve more and more of the original key. Here is a example of such a chunk: `' glow fro\x96ì@¬a1n'`. We can definitely say that the next 2 chararacters should be `m ` in order to get `glow from `. Eventually, if we repeat this process enough times, we can get the full key as an integer. This resulting key is `54098576040305148367357017600`. Using this in our original program gets us the full decrypted message, as seen below:

    Chapter 1 The Enigmatic Code
    In a dimly lit room in downtown Chicago, Ethan "Cipher" Reynolds sat hunched over his computer, the glow from the monitor casting a bluish hue across his determined face. The clock on the wall ticked past midnight, but Ethan paid no mind. He was deep into cracking one of the most intricate encryption systems hed ever encountered a challenge posted on an obscure forum known for its cryptographic puzzles. The flag hidden within, pctf{4_th3_tw0_t1m3_4a324510356}

    The challenge was simple in concept but daunting in execution decode a series of encrypted messages within a week. The reward was the kind of notoriety that could make or break a hackers reputation in the underground community. For Ethan, it was more than just fame; it was a calling.

    Chapter 2 The First Encounter
    Three days into the challenge, Ethan hit a wall. No matter how many algorithms he tried, the code wouldnt budge. Frustrated, he decided to take a break and scan the forum for any hints. As he browsed, a private message notification popped up.

    "Need help with the encryption? I can lend a hand. - DataPixie"

    Ethan was skeptical but curious. Hed heard of DataPixie, a mysterious figure known for their prowess in data analysis and cryptography. Reluctantly, he replied, "Sure, what do you have in mind?"

    Within minutes, DataPixie responded with a series of steps and a unique decryption tool. Ethan followed the instructions, and to his amazement, he made significant progress. Impressed, he invited DataPixie to join him on the project.

    Chapter 3 The Allies Assemble
    With DataPixie on board, Ethans confidence grew. They communicated constantly, sharing theories and bouncing ideas off each other. DataPixies real name was Lila, a brilliant programmer with a knack for finding patterns where others saw only chaos.

    As the duo worked, they realized they needed more expertise. Lila suggested contacting an old friend, a hardware specialist named Marco "BitNinja" Alvarez. Marcos deep understanding of hardware-based encryption could provide the edge they needed.

    Marco, intrigued by the challenge, joined without hesitation. His contribution was invaluable, offering insights into the physical layer of the encryption that neither Ethan nor Lila had considered.

    Chapter 4 The Roadblock
    Despite their combined efforts, the trio encountered a new roadblock. The encryption had layers within layers, and every time they peeled one back, another more complex one appeared. Their frustration grew, but giving up was not an option.

    Ethan decided to bring in one more person an old college friend and mathematician, Anya "ZeroDay" Patel. Anyas specialty was in theoretical mathematics and quantum computing. She had a reputation for thinking outside the box, which was precisely what they needed.

    Anya was intrigued by the challenge and joined the team. Her fresh perspective brought new life into their efforts. She suggested using quantum algorithms to tackle the encryption, a risky but potentially groundbreaking approach.

    Chapter 5 The Breakthrough
    With Anyas quantum techniques, the team made a breakthrough. They managed to decode a significant portion of the encrypted message, revealing what appeared to be coordinates. However, the coordinates were incomplete, leaving them with a tantalizing clue but no clear direction.

    Lila, ever the detective, suggested they investigate the partial coordinates. Using open-source intelligence tools, they traced the coordinates to a remote location in the Swiss Alps. It was an old, abandoned facility rumored to have been used for secretive projects during the Cold War.

    Chapter 6 The Journey
    Determined to uncover the truth, the team decided to meet in person for the first time and travel to Switzerland. They pooled their resources, and within days, they were on a plane, excitement and apprehension swirling within them.

    In the Alps, the journey to the abandoned facility was treacherous. Snow-covered paths and icy winds tested their resolve. But their determination never wavered. When they finally reached the facility, it was a derelict building half-buried in snow.

    Inside, they found old computers and documents. Among the relics was a dusty hard drive. Marcos skills came into play as he carefully extracted the data. To their astonishment, the drive contained the complete encrypted message they had been trying to crack.

    Chapter 7 The Revelation
    Back at their makeshift base, the team worked tirelessly to decrypt the final message. The combined expertise of Ethan, Lila, Marco, and Anya finally bore fruit. The message revealed a conspiracy that spanned decades, involving clandestine government operations and hidden caches of information.

    The knowledge they uncovered was both thrilling and terrifying. It implicated powerful individuals and exposed secrets that had been buried for years. They knew they had to tread carefully.

    Chapter 8 The Decision
    With the decrypted message in hand, the team faced a moral dilemma. Revealing the information could have far-reaching consequences, both good and bad. They debated long into the night, weighing the potential for justice against the chaos it could unleash.

    In the end, they decided to leak the information to trusted journalists known for their integrity. The story broke, causing a media frenzy. While the world grappled with the revelations, the team quietly went their separate ways, knowing they had made a difference.

    Epilogue The Legacy
    Ethan returned to Chicago, Lila to her data labs, Marco to his hardware ventures, and Anya to her quantum research. They stayed in touch, bound by the adventure they had shared. The challenge had brought them together, forging bonds that would last a lifetime.

    The encrypted odyssey was over, but the friendships they had formed and the skills they had honed remained. Wow. This is kind of a crazy story isn't it.
    0000000000

As we can see from this, our flag is right near the top: `pctf{4_th3_tw0_t1m3_4a324510356}`.


### My thoughts:
- Difficulty: 4/10
- Enjoyability: 7/10

I have never solved a cryptography challenge as hard as this, so I'm super proud of this one. It was a little tedious near the end, but it felt very nice to see the plaintext shot out of my code. I hope you enjoyed my first ever writeup!