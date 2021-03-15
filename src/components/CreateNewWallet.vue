<template>
  <v-dialog
    v-model="openDialog"
    max-width="400px"
    content-class="wallet-dialog"
    id="wallet-dialog"
    overlay-color="#050E18"
    overlay-opacity="0.8"
  >
    <template v-slot:activator="{ on, attrs }">
      <v-btn color="red lighten-2" dark v-bind="attrs" v-on="on">
        Create New Wallet
      </v-btn>
    </template>

    <section class="body">
      <div class="header">
        <div>Create New Wallet</div>
        <img @click="job.isDialog = false" src="../assets/close_btn.svg" />
      </div>
      <v-divider></v-divider>
      <div class="content" id="dialog-content">
        <div class="scan-content">
          <div class="head-txt">
            Your new Ethereum wallet was generated.<br />Save your private key
            in a safe place.
          </div>
          <div class="sec-txt">
            The wallet was generated in your browser. <br />You are the only one
            having access to the private key
          </div>
          <div><img src="../assets/qrscan.svg" /></div>
        </div>
        <v-divider></v-divider>
        <div class="form-content">
          <div>
            <div class="form-field">
              <div class="label-txt">Wallet address</div>
              <v-text-field background-color="rgba(0, 0, 0,0.3)" color="#FFF">
                <template slot="append">
                  <div class="append-items">
                    <v-img class="icon-select" src="../assets/copy.svg">
                    </v-img>
                  </div>
                </template>
              </v-text-field>
            </div>
            <div class="form-field form-pass">
              <div class="label-txt">Private Key</div>
              <v-text-field
                background-color="rgba(0, 0, 0,0.3)"
                color="#FFF"
                :type="showKey ? 'text' : 'password'"
              >
                <template slot="append">
                  <div class="append-items">
                    <v-img
                      class="icon-select"
                      @click="showKey = !showKey"
                      src="../assets/eye.svg"
                    >
                    </v-img>
                    <v-img class="icon-select" src="../assets/copy.svg">
                    </v-img>
                  </div>
                </template>
              </v-text-field>
            </div>
            <v-divider></v-divider>
            <div class="terms-txt">
              <img
                v-if="isAgree"
                @click="isAgree = !isAgree"
                src="../assets/checked.svg"
              />
              <div
                v-if="!isAgree"
                @click="isAgree = !isAgree"
                class="uncheckedbox"
              ></div>
              <div>
                I understand that EMDX has no access to my private key and in
                case of loss I will not be able to restore access to my wallet
                and the funds on the smart contracts.
              </div>
            </div>
          </div>
        </div>
      </div>
      <v-divider></v-divider>
      <div class="dlg-footer">
        <v-btn class="btn-footer" @click="openDialog = false">Ok</v-btn>
      </div>
    </section>
  </v-dialog>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "CreateNewWallet",
  data: () => {
    return {
      openDialog: false,
      showKey: false,
      isAgree: true,
    };
  },
});
</script>
<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
.wallet-dialog {
  background: #141c26;
  box-shadow: 0px 0px 100px rgba(0, 0, 0, 0.5);
  border-radius: 4px;

  .header {
    font-style: normal;
    font-weight: bold;
    font-size: 14px;
    line-height: 17px;
    color: #1262ff;
    font-family: "Roboto", sans-serif;
    height: 40px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    padding: 0px 15px;
    text-transform: uppercase;
    background: rgba(0, 0, 0, 0.2);
    img {
      width: 14px;
      height: 14px;
      cursor: pointer;
    }
  }
  .content {
    .scan-content {
      display: flex;
      flex-direction: column;
      text-align: center;
      padding: 18px 0px 24px 0px;
    }
    .head-txt {
      font-family: "Roboto", sans-serif;
      font-style: normal;
      font-weight: bold;
      font-size: 18px;
      line-height: 20px;
      padding-bottom: 10px;
    }
    .sec-txt {
      font-family: "Roboto", sans-serif;
      font-style: normal;
      font-weight: normal;
      font-size: 14px;
      line-height: 16px;
      text-align: center;
      padding-bottom: 24px;
      color: rgba($color: #ffffff, $alpha: 0.7);
    }
    .form-content {
      padding: 21px 20px 24px 20px;
      display: flex;
      flex-direction: column;
      gap: 20px;
      background-color: rgba(0, 0, 0, 0.2);
      .form-pass {
          padding-bottom: 29px;
      }
      .form-field {
        display: flex;
        flex-direction: column;
        gap: 3px;
        padding-bottom: 20px;
        .label-txt {
          font-family: "PT Sans";
          font-size: 12px;
          line-height: 16px;
          color: rgba($color: #ffffff, $alpha: 0.5);
        }
        .v-text-field {
          margin-top: 0px;
          padding: 0px;
        }
        .v-input__slot {
          padding: 0px 5px;
          margin-bottom: 0px;
          height: 34px !important;
          input {
            font-size: 12px;
            line-height: 14px;
            padding-left: 12px;
          }
          .v-input__control {
            height: 34px;
          }
          .v-input__append-inner {
            height: 100%;
            margin: 0px;
          }
          .v-text-field__details {
              min-height: 0px;
              height: 0px;
          }
          .append-items {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100%;
            flex-direction: row;
            gap: 10px;
            padding: right;
            padding: 0px 5px;
            .icon-select {
              cursor: pointer;
            }
          }
        }

        .v-input__slot:before {
          border-color: rgba(255, 255, 255, 0.1) !important;
        }
      }
    }
    .terms-txt {
      display: flex;
      flex-direction: row;
      align-items: flex-start;
      gap: 10px;
      padding: 29px 0px 0px 0;
      .uncheckedbox {
        min-width: 14px;
        min-height: 14px;
        background-color: #e33c50;
        border-radius: 2px;
      }
      div {
        font-family: "Roboto", sans-serif;
        font-style: normal;
        font-weight: normal;
        font-size: 10px;
        line-height: 12px;

        color: rgba(255, 255, 255, 0.7);
      }
    }
  }

  .dlg-footer {
    height: 70px;
    display: flex;
    align-items: center;
    justify-content: center;
    .btn-footer {
      background-color: rgba($color: #ffffff, $alpha: 0.1) !important;
      border-radius: 4px;
      font-style: normal;
      font-weight: bold;
      font-size: 14px;
      width: 150px;
      line-height: 16px;
      color: #fff;
      font-family: Roboto;
      letter-spacing: 0;
      height: 35px;
    }
  }
}
</style>