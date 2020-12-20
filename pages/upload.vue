<template>
    <v-row justify="center" align="center">
        <v-col cols="12" sm="10" class="d-flex flex-column align-center">
            <h2 class="form-title">Upload File</h2>

            <p>
                Please verify the file is valid as a wrong upload can lead to
                unexpected results
            </p>

            <FormulateInput
                type="file"
                name="file"
                label="Select your spreadsheet file"
                help=""
                validation="required"
                uploadBehavior="delayed"
                :uploader="uploadFile"
                v-model="document"
            />

            <v-btn
                :loading="uploading"
                :disabled="uploading"
                color="blue-grey"
                class="ma-2 white--text"
                @click="submitFIle"
            >
                Upload
                <v-icon right dark> mdi-cloud-upload </v-icon>
                <template v-slot:loader>
                    <span class="custom-loader">
                        <v-icon light>mdi-cached</v-icon>
                    </span>
                </template>
            </v-btn>
        </v-col>

        <v-snackbar v-model="uploaded">
            File has been uploaded

            <template v-slot:action="{ attrs }">
                <v-btn
                    color="pink"
                    text
                    v-bind="attrs"
                    @click="uploaded = false"
                >
                    Close
                </v-btn>
            </template>
        </v-snackbar>
    </v-row>
</template>

<script>
export default {
    head() {
        return {
            title: "Upload Events",
        };
    },

    name: "UploadPage",

    data() {
        return {
            document: null,

            uploading: false,

            uploaded: false,
        };
    },

    methods: {
        uploadFile: async function (file, progress, error, options) {
            this.uploading = true;
            try {
                const formData = new FormData();

                formData.append("file", file);

                await this.$axios.post("/events", formData);
                progress(100);
                this.uploaded = true;
                this.$router.replace("/");
                return {};
            } catch (err) {
                console.log(err);
                error("Unable to upload file");
            } finally {
                this.uploading = false;
            }
        },

        submitFIle: function () {
            this.document.upload();
        },
    },
};
</script>

<style>
.custom-loader {
    animation: loader 1s infinite;
    display: flex;
}
@-moz-keyframes loader {
    from {
        transform: rotate(0);
    }
    to {
        transform: rotate(360deg);
    }
}
@-webkit-keyframes loader {
    from {
        transform: rotate(0);
    }
    to {
        transform: rotate(360deg);
    }
}
@-o-keyframes loader {
    from {
        transform: rotate(0);
    }
    to {
        transform: rotate(360deg);
    }
}
@keyframes loader {
    from {
        transform: rotate(0);
    }
    to {
        transform: rotate(360deg);
    }
}
</style>