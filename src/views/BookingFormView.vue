<template>
    <div class="container">
        <form action="POST">
            <div class="kiri">
                <div class="form-wrapper">
                    <div>
                        <label for="">NAMA LENGKAP</label>
                        <input type="text" name="" id="">
                    </div>
                    <div>
                        <label for="">NOMOR TELEPON</label>
                        <input type="text" name="" id="">
                    </div>
                    <div>
                        <label for="">NOMOR KERABAT/ORANG TUA</label>
                        <input type="text" name="" id="">
                    </div>
                    <div>
                        <div>
                            <label for="">TANGGAL MASUK</label>
                            <input type="date" name="" id="">
                        </div>
                        <div>
                            <label for="">DURASI</label>
                            <select name="durations" id="durations">
                                <option value="30">1 Bulan</option>
                                <option value="365">1 Tahun</option>
                            </select>
                        </div>
                    </div>
                    <div style="border: none;">
                        <label for=""><b>UNGGAH FOTO KTP</b></label>
                        <input type="file" name="" id="" style="border: none;">
                    </div>
                </div>
            </div>
            <div class="kanan">

            </div>
        </form>
    </div>
</template>

<script>

</script>

<style>
.kiri{
    width: 50%;
    padding: 20px 20px;

}
.form-wrapper{
    border: 1px solid black;
    font-weight: bold;
    color: rgba(0, 0, 0, 0.325);
}
.form-wrapper > div{
    display: flex;
    padding:10px 10px;
}
.form-wrapper > div > div{
    display: flex;
}
.form-wrapper input{
    width: 350px;
}
.kanan{
    width: 50%;
}
</style>