<!-- src/App.svelte -->
<svelte:head>
  <title>Ionic Companion - svelte power!</title>
</svelte:head>

  <ion-split-pane content-id="main">
    <Menu />
    <div class="ion-page" id="main">
      <slot />
    </div>
  </ion-split-pane>


<style>

</style>

<script>
import { IonicShowModal } from "../services/IonicControllers";

import localforage from "localforage";

import SourceViewer from "../components/SourceViewer.svelte";
import Menu from "../components/Menu.svelte";

import CookieJar from "../components/CookieJar.svelte";
import RateMe from "../components/RateMe.svelte";

// source viewer and pulsating class for the FAB
let pulseSourceViewer = false;
localforage.getItem("has-seen-source").then((value) => {
  console.log("has-seen-source", value);
  if (!value) {
    // let's not trigger the user immmediately
    setTimeout(() => {
      pulseSourceViewer = true;
      // and stop the pulse after 1 minute
      setTimeout(() => {
        pulseSourceViewer = false;
      }, 60000);
    }, 20000);
  }
});

// handler for the sourceviewer
const viewSource = () => {
  localforage.setItem("has-seen-source", true);
  pulseSourceViewer = false;

  IonicShowModal("source-viewer", SourceViewer, {
    name: window.location.pathname,
  });
};
</script>
