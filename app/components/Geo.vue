<template>
  <Page>
    <ActionBar title="Geolocation">
      <NavigationButton
        @tap="$navigateBack"
        text="Go Back"
        android.systemIcon="ic_menu_back"
      />
    </ActionBar>

    <StackLayout>
      <Button
        @tap="getLocation"
        class="btn"
        text="Get Current Location"
      />

      <ActivityIndicator :busy="isBusy" />

      <StackLayout v-if="locationInfo.length > 0">
        <ListView for="item in locationInfo">
          <v-template>
            <FlexboxLayout class="info">
              <Label :text="item.key" class="key" />
              <Label :text="item.value" class="value" />
            </FlexboxLayout>
          </v-template>
        </ListView>
      </StackLayout>

      <StackLayout v-else>
        <Label text="Click the button for location info" />
      </StackLayout>
    </StackLayout>
  </Page>
</template>

<script>
import { objectToObjectArray } from '../utils';
import * as geolocation from 'nativescript-geolocation';
import { Accuracy } from 'tns-core-modules/ui/enums';

export default {
  data() {
    return {
      isBusy: false,
      locationInfo: [],
    };
  },
  methods: {
    getLocation() {
      this.isBusy = true;

      geolocation.getCurrentLocation({
        desiredAccuracy: Accuracy.high,
        updateDistance: 10,
        maximumAge: 20000,
        timeout: 20000,
      }).then(loc => {
        this.isBusy = false;

        if (loc) {
          this.locationInfo = objectToObjectArray(loc);
        }
      }, e => console.error(`Error: ${e.message}`));
    },
  },
};
</script>

<style lang="scss" scoped>
.info {
  @include flex-between;

  .key {
    text-transform: capitalize;
  }

  .value {
    color: $grey;
  }
}
StackLayout > StackLayout {
  margin: 0;
}
Label {
  font-size: 20;
  padding: 10 0;
}
</style>
