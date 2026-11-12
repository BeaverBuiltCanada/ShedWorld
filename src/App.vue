<!--  this is a Vue/HTML comment -->
/*=================THIS IS HOW YOU COMMENT OUT======================
MULTI-LINES==========================================================
====================================================*/

<script setup>
import { ref, computed, watch } from "vue";
import JSZip from "jszip";
import roofPeakWidth from "./assets/roofPeakWidth.png";
import roofPeakDepth from "./assets/roofPeakDepth.png";
import twoIso from "./assets/two_iso.png";
import skidsShowing from "./assets/skidsShowing.png";
import tapeMeasure from "./assets/tapeMeasureBorder.png";
import frontDoorReferenceImage from "./assets/Front-Door-Reference-Image-v1.png";
import "@fontsource/inter";
import "@fontsource/oswald";

/*===================UTILITY FUNCTIONS==================*/

function acceptTerms() {
  acceptedTerms.value = true;

  window.scrollTo({
    top: 0,
    behavior: "instant",
  });
}

const acceptedTerms = ref(false);
const showFAQ = ref(false);
const showTerms = ref(false);
const showWelcomeDetails = ref(true);
const buildingType = ref("");
const topPlateThickness = 1.5; /*============WINDOW HEIGHT WARNING USES THIS======*/

/*=========================================================DIMENSIONS=============*/
const width = ref(12); // Default width is 12 feet before any selection made.
const showWhatIsWidth = ref(false);
const depth = ref(12); // Default depth is 12 feet before any selection made.
const showWhatIsDepth = ref(false);
const wallHeight = ref(8); // Default wall height is 8 feet before any selection made.
const showWhatIsWallHeight = ref(false);
const floorArea = computed(() => width.value * depth.value);
const perimeter = computed(() => width.value * 2 + depth.value * 2);
const wallArea = computed(() => perimeter.value * wallHeight.value);

/*=============================================FOUNDATION SKIDS===================*/
const showWhatIsASkid = ref(false);
const skidType = ref("");
const centerSkid = ref("");
/*==============================================FLOOR JOISTS===================*/
const showWhatIsAJoist = ref(false);
const floorJoistSize = ref("");
const show2x4FloorJoistInfo = ref(false);
const show2x6FloorJoistInfo = ref(false);
/*=============================================FLOOR PLYWOOD==========*/
const floorPlywoodThickness = ref("2"); // Default floor plywood thickness should be set to 5/8"
const plywoodDisplay = computed(() => {
  switch (floorPlywoodThickness.value) {
    case "1":
      return '1/2"';
    case "2":
      return '5/8"';
    case "3":
      return '3/4"';
    case "4":
      return '1"';
    default:
      return floorPlywoodThickness.value;
  }
});
/*====================================================WALL STUDS=================*/
/*=========================
== STUDS DISPLAY==========*/
const studSpacingOverrideDisplay = computed(() => {
  return wallStudSpacingOverride.value ? "Included" : "Not Included";
});
const showWhatIsAStud = ref(false);
const wallStudSize = ref("");
const showMoreAbout2x4Studs = ref(false);
const showMoreAbout2x6Studs = ref(false);
const showAdvancedStudOptions = ref(false);
const wallStudSpacingOverride = ref(false);
const wallStudOverrideSpacing = ref(24);
const currentStudSpacing = computed(() => {
  if (wallStudSpacingOverride.value) {
    return wallStudOverrideSpacing.value;
  }
  return wallStudSize.value === "2x6" ? 24 : 16;
});
/*====================================================ROOF=======================*/
const roofPeakDirection = ref("");
const showMoreAboutRoofPeak = ref(false);
const showWhatIsARoofRafter = ref(false);
const showMoreAbout2x4RoofRafters = ref(false);
const showMoreAbout2x6RoofRafters = ref(false);
const roofRafterSize = ref("");
const showWhatIsRoofPitch = ref(false);
const roofPitch = ref("6"); // Default roof pitch should be set to 6:12
const showWhatAreEaves = ref(false);
const eavesOverHang = ref(6); // Default eaves overhang is 6 inches
const showWhatAreGableEnds = ref(false);
const dimensionWarning = computed(() => {
  return Number(width.value) > Number(depth.value);
});
/*====================================================== FRONT DOOR SECTION (RO3)========
===================================================== stands for rough opening 3 =======
===================================================== this is in Fusion front door====*/
/*====================
== FRONT DOOR DISPLAYS
======================*/

const frontDoorDisplay = computed(() => {
  return frontDoor.value === 1 ? "Included" : "Not Installed";
});

const frontDoorDistanceDisplay = computed(() => {
  return frontDoorStudIndex.value === 0
    ? "Centered"
    : `${frontDoorStudIndex.value}"`;
});

const frontDoorHeaderSizeDisplay = computed(() => {
  return frontDoorHeaderSize.value === "2x6" ? "2x6 Header" : "2x4 Header";
});

/*====================
== FRONT DOOR INPUTS
======================*/

const showWhatIsAFrontDoor = ref(false);
const showMoreAboutFrontDoor = ref(false);

const frontDoor = ref(0);

const frontDoorWidth = ref(36);

const frontDoorHeight = ref(80);

const frontDoorFloorHeight = ref(0);

const frontDoorHeaderSize = ref("2x6");

const frontDoorStudIndex = ref(0);

/*=====================
== FRONT DOOR WARNINGS
=======================*/
// width warning
const frontDoorWidthWarning = computed(() => {
  const wallLengthInches = Number(width.value) * 12;

  return Number(frontDoorWidth.value) > wallLengthInches - 10;
});
// height warning
const frontDoorFitWarning = computed(() => {
  const wallHeightInches = Number(wallHeight.value) * 12;

  const headerHeight = frontDoorHeaderSize.value === "2x6" ? 5.5 : 3.5;

  const requiredHeight =
    Number(frontDoorFloorHeight.value) +
    Number(frontDoorHeight.value) +
    headerHeight +
    topPlateThickness;

  return requiredHeight > wallHeightInches;
});

// distance from corner warning
const frontDoorWarning = computed(() => {
  if (Number(frontDoorStudIndex.value) === 0) {
    return false;
  }

  const wallLengthInches = Number(width.value) * 12;
  const cornerClearance = 12;

  return (
    Number(frontDoorStudIndex.value) +
      Number(frontDoorWidth.value) +
      cornerClearance >
    wallLengthInches
  );
});

/*====================================================== LEFT WALL WINDOW (RO2)========
===================================================== stands for rough opening 2 =======
===================================================== this is in Fusion left wall=====*/
/*====================
==LEFT WINDOW DISPLAYS*/
const leftWallRO2WindowDisplay = computed(() => {
  return leftWallRO2Window.value === 1 ? "Included" : "Not Installed";
});

const leftWallRO2StudIndexDisplay = computed(() => {
  return leftWallRO2StudIndex.value === 0
    ? "Centered"
    : `${leftWallRO2StudIndex.value}"`;
});

const leftWallRO2HeaderSizeDisplay = computed(() => {
  return leftWallRO2HeaderSize.value === "2x6" ? "2x6 Header" : "2x4 Header";
});

const showWhatIsALeftWallWindow = ref(false);
const showMoreAboutLeftWallWindow = ref(false);
/*===================
==LEFT WINDOW INPUTS*/
const leftWallRO2Window = ref(0);
const leftWallRO2Width = ref(24);
const leftWallRO2Height = ref(24);
const leftWallRO2FloorHeight = ref(10);
const leftWallRO2HeaderSize = ref("2x4");
const leftWallRO2StudIndex = ref(0);

/*=====================
==LEFT WINDOW WARNINGS*/
const leftWindowWidthWarning = computed(() => {
  const wallLengthInches = Number(depth.value) * 12;

  return Number(leftWallRO2Width.value) > wallLengthInches - 10;
});

const leftWindowFitWarning = computed(() => {
  const wallHeightInches = Number(wallHeight.value) * 12;

  const headerHeight = leftWallRO2HeaderSize.value === "2x6" ? 5.5 : 3.5;

  const requiredHeight =
    Number(leftWallRO2FloorHeight.value) +
    Number(leftWallRO2Height.value) +
    headerHeight +
    topPlateThickness;

  return requiredHeight > wallHeightInches;
});

const leftWindowWarning = computed(() => {
  if (Number(leftWallRO2StudIndex.value) === 0) {
    return false;
  }

  const wallLengthInches = Number(depth.value) * 12;
  const cornerClearance = 12;

  return (
    Number(leftWallRO2StudIndex.value) +
      Number(leftWallRO2Width.value) +
      cornerClearance >
    wallLengthInches
  );
});
/*============================================= RIGHT WALL WINDOW (RO4)============
================================================ stands for rough opening 4 =======
============================================== this is in Fusion right wall=============*/
/*====================
==RIGHT WINDOW DISPLAYS*/
const rightWallRO4WindowDisplay = computed(() => {
  return rightWallRO4Window.value === 1 ? "Included" : "Not Installed";
});

const rightWallRO4StudIndexDisplay = computed(() => {
  return rightWallRO4StudIndex.value === 0
    ? "Centered"
    : `${rightWallRO4StudIndex.value}"`;
});

const rightWallRO4HeaderSizeDisplay = computed(() => {
  return rightWallRO4HeaderSize.value === "2x6" ? "2x6 Header" : "2x4 Header";
});

/*===================
==RIGHT WINDOW INPUTS*/
const rightWallRO4Window = ref(1);
const rightWallRO4Width = ref(24);
const rightWallRO4Height = ref(24);
const rightWallRO4FloorHeight = ref(10);
const rightWallRO4HeaderSize = ref("2x4");
const rightWallRO4StudIndex = ref(0);

const showWhatIsARightWallWindow = ref(false);
const showMoreAboutRightWallWindow = ref(false);

/*=====================
==RIGHT WINDOW WARNINGS*/
const rightWindowWidthWarning = computed(() => {
  const wallLengthInches = Number(depth.value) * 12;

  return Number(rightWallRO4Width.value) > wallLengthInches - 10;
});

const rightWindowFitWarning = computed(() => {
  const wallHeightInches = Number(wallHeight.value) * 12;

  const headerHeight = rightWallRO4HeaderSize.value === "2x6" ? 5.5 : 3.5;

  const requiredHeight =
    Number(rightWallRO4FloorHeight.value) +
    Number(rightWallRO4Height.value) +
    headerHeight +
    topPlateThickness;

  return requiredHeight > wallHeightInches;
});

const rightWindowWarning = computed(() => {
  if (Number(rightWallRO4StudIndex.value) === 0) {
    return false;
  }

  const wallLengthInches = Number(depth.value) * 12;
  const cornerClearance = 12;

  return (
    Number(rightWallRO4StudIndex.value) +
      Number(rightWallRO4Width.value) +
      cornerClearance >
    wallLengthInches
  );
});
/*===LEGACY - AUDIT BEFORE DELETING===BECAUSE I MADE const currentStudSpacing AFTER THIS=============
const wallStudSpacing = computed(() => {
  return wallStudSize.value === "2x6" ? 24 : 16;
});=====================*/

/*=================================================END OF RIGHT WINDOW=================*/

/*=============================THESE SET DEFAULTS FOR EACH SELECTED BUILDING TYPE==========*/
function selectBuildingType(type) {
  buildingType.value = type;
  {
    showWelcomeDetails.value = false;

    // presets...
  }

  if (type === "animalHousing") {
    width.value = 4;
    depth.value = 4;
    wallHeight.value = 4;
    skidType.value = "2x4flat";
    centerSkid.value = "";
    floorJoistSize.value = "2x4";
    floorPlywoodThickness.value = "1";
    wallStudSize.value = "2x4";
    roofPeakDirection.value = "depth";
    roofRafterSize.value = "2x4";
    roofPitch.value = "4";
    eavesOverHang.value = "4";

    frontDoor.value = 1;
    frontDoorWidth.value = 24;
    frontDoorHeight.value = 24;
    frontDoorFloorHeight.value = 0;
    frontDoorHeaderSize.value = "2x4";
    frontDoorStudIndex.value = 0;

    leftWallRO2Window.value = 1;
    leftWallRO2Width.value = 12;
    leftWallRO2Height.value = 12;
    leftWallRO2FloorHeight.value = 12;
    leftWallRO2HeaderSize.value = "2x4";
    leftWallRO2StudIndex.value = 0;

    rightWallRO4Window.value = 0;
    rightWallRO4Width.value = 12;
    rightWallRO4Height.value = 12;
    rightWallRO4FloorHeight.value = 12;
    rightWallRO4HeaderSize.value = "2x4";
    rightWallRO4StudIndex.value = 0;
  }

  if (type === "yardShed") {
    width.value = 8;
    depth.value = 8;
    wallHeight.value = 6;
    skidType.value = "2x4flat";
    centerSkid.value = "";
    floorJoistSize.value = "2x4";
    floorPlywoodThickness.value = "2";
    wallStudSize.value = "2x4";
    roofPeakDirection.value = "depth";
    roofRafterSize.value = "2x4";
    roofPitch.value = "6";
    eavesOverHang.value = "6";

    frontDoor.value = 1;
    frontDoorWidth.value = 60;
    frontDoorHeight.value = 60;
    frontDoorFloorHeight.value = 0;
    frontDoorHeaderSize.value = "2x4";
    frontDoorStudIndex.value = 0;

    leftWallRO2Window.value = 1;
    leftWallRO2Width.value = 24;
    leftWallRO2Height.value = 24;
    leftWallRO2FloorHeight.value = 24;
    leftWallRO2HeaderSize.value = "2x4";
    leftWallRO2StudIndex.value = 0;

    rightWallRO4Window.value = 1;
    rightWallRO4Width.value = 24;
    rightWallRO4Height.value = 24;
    rightWallRO4FloorHeight.value = 24;
    rightWallRO4HeaderSize.value = "2x4";
    rightWallRO4StudIndex.value = 0;
  }

  if (type === "workshopGarage") {
    width.value = 12;
    depth.value = 8;
    wallHeight.value = 8;
    skidType.value = "4x4";
    centerSkid.value = "Use Center Skid";
    floorJoistSize.value = "2x6";
    floorPlywoodThickness.value = "4";
    wallStudSize.value = "2x6";
    roofPeakDirection.value = "depth";
    roofRafterSize.value = "2x6";
    roofPitch.value = "8";
    eavesOverHang.value = "8";

    frontDoor.value = 1;
    frontDoorWidth.value = 36;
    frontDoorHeight.value = 80;
    frontDoorFloorHeight.value = 0;
    frontDoorHeaderSize.value = "2x6";
    frontDoorStudIndex.value = 0;

    leftWallRO2Window.value = 1;
    leftWallRO2Width.value = 48;
    leftWallRO2Height.value = 32;
    leftWallRO2FloorHeight.value = 33;
    leftWallRO2HeaderSize.value = "2x6";
    leftWallRO2StudIndex.value = 0;

    rightWallRO4Window.value = 1;
    rightWallRO4Width.value = 48;
    rightWallRO4Height.value = 32;
    rightWallRO4FloorHeight.value = 33;
    rightWallRO4HeaderSize.value = "2x6";
    rightWallRO4StudIndex.value = 0;
  }

  if (type === "customStructure") {
    width.value = 24;
    depth.value = 24;
    wallHeight.value = 10;
    skidType.value = "4x4";
    centerSkid.value = "Use Center Skid";
    floorJoistSize.value = "2x6";
    floorPlywoodThickness.value = "4";
    wallStudSize.value = "2x6";
    roofPeakDirection.value = "depth";
    roofRafterSize.value = "2x6";
    roofPitch.value = "6";
    eavesOverHang.value = "16";

    frontDoor.value = 1;
    frontDoorWidth.value = 108; /*single garage door*/
    frontDoorHeight.value = 96; /* but double check, once confirmed add note here.*/
    frontDoorFloorHeight.value = 0;
    frontDoorHeaderSize.value = "2x6";
    frontDoorStudIndex.value = 0;

    leftWallRO2Window.value = 1;
    leftWallRO2Width.value = 60;
    leftWallRO2Height.value = 48;
    leftWallRO2FloorHeight.value = 30;
    leftWallRO2HeaderSize.value = "2x6";
    leftWallRO2StudIndex.value = 0;

    rightWallRO4Window.value = 1;
    rightWallRO4Width.value = 60;
    rightWallRO4Height.value = 48;
    rightWallRO4FloorHeight.value = 30;
    rightWallRO4HeaderSize.value = "2x6";
    rightWallRO4StudIndex.value = 0;
  }
  window.scrollTo({
    top: 0,
    behavior: "instant",
  });
}
/*==========================================EXPORT JSON=============================*/
async function loadTheMechanism() {
  const config = {
    width: Number(width.value),
    depth: Number(depth.value),
    wallHeight: Number(wallHeight.value),
    skidType: skidType.value,
    centerSkid: centerSkid.value,
    floorJoistSize: floorJoistSize.value,
    floorPlywoodThickness: floorPlywoodThickness.value,
    wallStudSize: wallStudSize.value,
    wallStudSpacingOverride: wallStudSpacingOverride.value,
    wallStudOverrideSpacing: wallStudOverrideSpacing.value,
    roofPeakDirection: roofPeakDirection.value,
    roofRafterSize: roofRafterSize.value,
    roofPitch: roofPitch.value,
    eavesOverHang: eavesOverHang.value,

    frontDoor: frontDoor.value,
    frontDoorWidth: frontDoorWidth.value,
    frontDoorHeight: frontDoorHeight.value,
    frontDoorFloorHeight: frontDoorFloorHeight.value,
    frontDoorHeaderSize: frontDoorHeaderSize.value,
    frontDoorStudIndex: frontDoorStudIndex.value,

    leftWallRO2Window: leftWallRO2Window.value,
    leftWallRO2Width: leftWallRO2Width.value,
    leftWallRO2Height: leftWallRO2Height.value,
    leftWallRO2FloorHeight: leftWallRO2FloorHeight.value,
    leftWallRO2HeaderSize: leftWallRO2HeaderSize.value,
    leftWallRO2StudIndex: leftWallRO2StudIndex.value,
    rightWallRO4Window: rightWallRO4Window.value,
    rightWallRO4Width: rightWallRO4Width.value,
    rightWallRO4Height: rightWallRO4Height.value,
    rightWallRO4FloorHeight: rightWallRO4FloorHeight.value,
    rightWallRO4HeaderSize: rightWallRO4HeaderSize.value,
    rightWallRO4StudIndex: rightWallRO4StudIndex.value,
  };

  const json = JSON.stringify(config, null, 2);
  /*==Right now, when you click LOAD THE MECHANISM™, your Vue app is doing the const blob block below============*/
  const zip = new JSZip();

  zip.file("config.json", json);

  const zipBlob = await zip.generateAsync({
    type: "blob",
  });

  const url = URL.createObjectURL(zipBlob);

  const link = document.createElement("a");

  link.href = url;
  link.download = "BeaverBuiltConfiguration.zip";

  link.click();

  URL.revokeObjectURL(url);
}
/*=========and that is what launches the Save / Save As experience.===*/
</script>

<template>
  <div v-if="!acceptedTerms">
    <!-- ============================Landing PAGE 1====WELCOME====FAQ===Terms & Disclaimer=========
     ============================================================-->
    <div class="landingPage1">
      <div class="heroHeader">
        <h1>Welcome to Beaver Built Canada</h1>
        <h2>Custom Building Configurator</h2>
      </div>
      <p>
        Design your own shed, workshop, garage, animal shelter, or outbuilding
        using standard Canadian dimensional lumber and generate a customized set
        of downloadable construction plans and cut lists based on your
        selections.
      </p>
      <p>
        Designs are generated using North American building conventions,
        including imperial measurements and standard dimensional lumber sizing
        (for example, a nominal 2×4 is treated as 1½" × 3½" actual size.
        Likewise a nominal 2×6 is treated as 1½" × 5½" actual size).
      </p>
      <h3>Before You Begin:</h3>
      <!--Start of BULLET POINTS on LANDING PAGE-->
      <ul>
        <li>
          These are emailed or downloadable plans and cut lists intended for
          personal projects, not free downloads.
        </li>
        <li>
          Plans are generated from the selections you make within this
          configurator.
        </li>
        <li>
          No lumber, hardware, building kits, or construction services are
          included.
        </li>
        <li>
          You are responsible for verifying local building codes, permit
          requirements, zoning restrictions, setbacks, site conditions, snow
          loads, wind loads, and all other local requirements before
          construction.
        </li>
      </ul>

      <!--Start of FAQ-->
      <div>
        <p>
          <button @click="showFAQ = !showFAQ">
            Frequently Asked Questions
          </button>
        </p>
        <div v-if="showFAQ">
          <p>
            <strong class="faq-q">
              Q: Is this configurator difficult or technical to use?</strong
            ><br />
            A: No, this configurator is designed to be simple and user-friendly,
            for all level skilled builders, yet it is robust for advanced
            builders. It has
            <button @click="showMoreInfoExample = !showMoreInfoExample">
              More Info
            </button>
            and
            <button @click="showHelpExample = !showHelpExample">Help</button>
            along the way, should you prefer to see them.
          </p>
          <P
            ><strong class="faq-q">
              Q:What kind of building material can I choose using this
              configurator?</strong
            ><br />
            A: This configurator uses 2x4 and 2x6 standard dimensional lumber.
            Also 4x4 in some cases.
          </P>
          <P
            ><strong class="faq-q">
              Q:What are some of the more advanced features using this
              configurator, that an experienced builder might find
              useful?</strong
            ><br />
            A: Some of the more advanced features include the ability to choose
            different sizes of lumber for floor joists, wall studs, and roof
            rafters. You can also choose to use a center skid for larger
            structures, and you can specify the roof pitch and peak direction.
            Additionally, you can customize the dimensions of your building to
            fit your specific needs. You can choose rough opening sizes for
            windows and doors, what size of headers you want to use and you can
            also place the openings by stud index and height from floor.
          </P>
          <p>
            <strong class="faq-q"> Q: Are these plans free?</strong><br />
            A: No, these are emailed or downloadable plans and cut lists for
            personal projects.
          </p>
          <p>
            <strong class="faq-q"> Q: How much do the plans cost?</strong><br />
            A: The cost of the plans is (yet to be determined, maybe $20. CAD)
            for a single building design and cut list. This is a one-time fee
            for the plans and cut list for your specific building design. You
            will receive a link to download the plans and cut list after payment
            is received. You will also receive a link to download the plans and
            cut list in your email.
          </p>
          <p>
            <strong class="faq-q">
              Q: What is the early builders honor club?</strong
            ><br />
            A: The early builders honor club is a group of early adopters who
            receive free plans and cut lists for testing and feedback. They have
            also agreed to allow Beaver Built Canada to use their projects,
            pictures and their feedback on the BeaverBuiltCanada.ca website and
            for marketing purposes. This is based on the honor system, and their
            feedback is greatly appreciated and helps drive improvements for
            everybody.
          </p>
          <p>
            These are real builders of designs they made themselves using this
            configurator; plans and cut lists. You can see their testimonials by
            clicking this link:
          </p>
          <p>
            Should you wish to join the early builders honor club, please
            contact us at: (if the link is active, we are looking for early
            builders to test and provide feedback on this configurator) and tell
            them, "Beaver sent me".
          </p>
          <p>
            <strong class="faq-q"> Q: Do you provide lumber or hardware?</strong
            ><br />
            A: No, this configurator generates plans and cut lists only. You
            will need to source your own materials.
          </p>
          <p>
            <strong class="faq-q">
              Q: Can I use these plans for commercial purposes?</strong
            ><br />
            A: No, these plans are intended for personal use only. Commercial
            use is not recommended.
          </p>
        </div>
      </div>
      <!--Start of VIEW FULL DISCLAIMER-->
      <p>
        <button @click="showTerms = !showTerms">View Full Disclaimer</button>
      </p>
      <div v-if="showTerms">
        <p>
          BeaverBuilt Canada provides downloadable DIY building plans, drawings,
          and cut lists for informational purposes only.
        </p>
        <p>
          The plans provided are intended as general building guidance and may
          not be suitable for all locations, conditions, climates, or intended
          uses.
        </p>
        <p>
          The builder is solely responsible for verifying compliance with all
          applicable building codes, permit requirements, zoning regulations,
          property setbacks, utility easements, environmental restrictions, snow
          loads, wind loads, and site-specific conditions before construction
          begins.
        </p>
        <p>
          BeaverBuilt Canada does not provide engineering certification, permit
          approval, code review, site inspection, construction supervision, or
          professional engineering services.
        </p>
        <p>
          All plans are provided as-is without warranties or guarantees of
          fitness for a particular purpose.
        </p>
        <p>
          BeaverBuilt Canada shall not be liable for construction errors,
          omissions, material costs, project delays, code-compliance issues,
          permit denials, property damage, personal injury, or any direct or
          indirect losses arising from the use of these plans.
        </p>
        <p>
          By using this configurator, downloading plans, or constructing from
          those plans, the builder accepts full responsibility for the resulting
          project.
        </p>
      </div>
      <div class="acceptance-box">
        <p>
          By continuing, you acknowledge that you have read and understood the
          information above and accept the full disclaimer.
        </p>

        <input type="checkbox" :checked="acceptedTerms" @change="acceptTerms" />

        <label> I understand and accept these terms. </label>
      </div>
    </div>
  </div>
  <!--========End Page 1=======terms are accepted========Start of PAGE 2 LANDING=========
  ==============================Branding Header====Configurator v1.0=============-->
  <div v-if="acceptedTerms">
    <div class="landingPage2">
      <!-- Main Building Type selection for Configurator -->
      <!-- Top of page 2 landing -->
      <!--<h1>WELCOME TO:</h1>-->
      <h2 v-if="!buildingType" class="branding-header page2-brandingHeader">
        Beaver Built Canada: Custom Building Configurator, v1.0
      </h2>
      <div v-if="!buildingType" class="sticky-tape">
        <img :src="tapeMeasure" alt="Tape Measure" class="tape_measure_image" />
      </div>
      <button
        v-if="!buildingType"
        @click="showWelcomeDetails = !showWelcomeDetails"
      >
        {{
          showWelcomeDetails
            ? "▼ Thanks For Visiting!"
            : "▶ Thanks For Visiting!"
        }}
      </button>
      <div v-if="showWelcomeDetails" class="welcome-details">
        <p>
          🦫 We have designed this configurator to be simple, enjoyable and
          user-friendly for builders of all skill levels. Whether this is your
          first building project or you've spent years working in the trades,
          the goal is to make designing your building straightforward and
          enjoyable. For experienced builders, additional options and advanced
          features are available to provide greater control over the final
          design.
        </p>
        <!--=====================This is currently a link to a photo for development phase=========-->
        <p>
          Visit
          <a
            href="https://beaverbuiltcanada.github.io/memories/trial-chair/"
            target="_blank"
            rel="noopener"
            >BeaverBuiltCanada on github</a
          >
          to read the documentation and configurator help topics (some
          documentation may not be available as work in progress, please check
          back soon). If you require additional help, feel free to contact us by
          email at: 🦫
        </p>
      </div>
      <!-- ==========================WHAT WOULD YOU LIKE TO BUILD?============================
     ==============================top of building-type cards of the class selectBuildingType====
     ================================================== -->

      <div v-if="!buildingType">
        <h3 class="pickYourPoison">What Would You Like To Build?</h3>
        <div
          class="scopeBuilding-card"
          @click="selectBuildingType('animalHousing')"
        >
          <h3>🐾 Animal & Pet Housing 🐓 🐎</h3>
          <p>
            Dog houses, chicken coops, shelters and other small outdoor
            structures.
          </p>
        </div>

        <div class="scopeBuilding-card" @click="selectBuildingType('yardShed')">
          <h3>🏡 Yard Shed 🌱 🚜</h3>
          <p>
            Storage sheds, garden sheds, utility buildings, and general backyard
            storage projects.
          </p>
        </div>

        <div
          class="scopeBuilding-card"
          @click="selectBuildingType('workshopGarage')"
        >
          <h3>🔨 Workshop & Garage 🛞 🏗️</h3>
          <p>
            Workshops, garages, hobby spaces, and larger utility structures.
          </p>
        </div>

        <div
          class="scopeBuilding-card"
          @click="selectBuildingType('customStructure')"
        >
          <h3>📐 Custom Structure 🏎️ 🚀</h3>
          <p>
            Start with a blank configuration and build exactly what you need.
          </p>
        </div>
      </div>
    </div>
    <!--=======================End of page 2=======building type selectors=========== -->
    <!--==========This is where 2 column layout should start=========-->
    <!-- Start of Building Dimensions above two orthographic views with view cubes -->

    <div v-if="buildingType" class="configurator-layout">
      <div class="landingPage3">
        <h2 class="branding-header page3-brandingHeader">
          Beaver Built Canada: Custom Building Configurator, v1.0
        </h2>

        <button
          class="collapse-thanks-button"
          @click="showWelcomeDetails = !showWelcomeDetails"
        >
          {{
            showWelcomeDetails
              ? "▼ Thanks For Visiting!"
              : "▶ Thanks For Visiting!"
          }}
        </button>
        <div
          v-if="showWelcomeDetails"
          class="welcome-details left-welcome-details"
        >
          <p>Thank you for using the Beaver Built Canada configurator.</p>
        </div>

        <div class="targeted-building-container">
          <div class="targetedBuilding-card">
            <a
              href="#"
              class="change-building-link"
              @click.prevent="buildingType = ''"
              >← Choose a different building type</a
            >
            <h3 v-if="buildingType === 'animalHousing'">
              🐾 Animal &amp; Pet Housing 🐓 🐎
            </h3>
            <h3 v-else-if="buildingType === 'yardShed'">🏡 Yard Shed 🌱 🚜</h3>
            <h3 v-else-if="buildingType === 'workshopGarage'">
              🔨 Workshop &amp; Garage 🛞 🏗️
            </h3>
            <h3 v-else>📐 Custom Structure 🏎️ 🚀</h3>
            <img
              :src="tapeMeasure"
              alt="Tape Measure"
              class="page3-tape-image"
            />
          </div>
        </div>
        <!--======================================TOP OF CONTROL PANEL=====================-->
        <aside class="page3-sidebar">
          <div class="page3-control-panel">
            <div class="dashboard-header">
              <h2>DASHBOARD</h2>
              <h4>Your Current Building Selections:</h4>
            </div>
            <div class="dashboard-intro">
              <p>(Updates automatically as you make selections.)</p>
              <ol>
                <li>After completing your design, click LOAD THE MECHANISM™</li>
                <li>Save the ZIP file somewhere you can easily find it</li>
                <li>Continue to Submission</li>
                <li>Upload your ZIP file for review</li>
              </ol>
            </div>
            <!--=====================DASHBOARD BUILDING FOOTPRINT DIMENSIONS===============-->
            <div class="dimensions-summary-section">
              <div class="CP-summary-row">
                <span>Depth:</span><span>{{ depth }}'</span>
              </div>
              <div class="CP-summary-row">
                <span>Width:</span><span>{{ width }}'</span>
              </div>
              <div class="CP-summary-row">
                <span>Wall Height:</span><span>{{ wallHeight }}'</span>
              </div>
            </div>
            <!--=====================DASHBOARD FOUNDATION===============-->
            <div class="foundation-summary-section">
              <div class="CP-summary-row">
                <span>Foundation Skids:</span>
                <span>{{ skidType }}</span>
              </div>
              <div class="CP-summary-row">
                <span>Center Skid:</span>
                <span>{{ centerSkid || "No" }}</span>
              </div>
            </div>
            <!--====================================DASHBOARD FLOOR JOISTS===============-->
            <div class="floor-joist-summary-section">
              <div class="CP-summary-row">
                <span>Floor Joists:</span>
                <span>{{ floorJoistSize }}</span>
              </div>
            </div>
            <!--==================================DASHBOARD FLOOR PLYWOOD===============-->
            <div class="plywood-summary-section">
              <div class="CP-summary-row">
                <span>Floor Plywood:</span>
                <span>{{ plywoodDisplay }}</span>
              </div>
            </div>
            <!--===============================================DASHBOARD WALL STUDS===============-->
            <!--======================THIS IS THE START OF THE ADVANCED STUD SYSTEM===-->
            <div class="stud-summary-section">
              <div class="CP-summary-row">
                <span>Wall Studs:</span>
                <span>{{ wallStudSize }}</span>
              </div>

              <div class="CP-summary-row">
                <span>Stud Spacing Override:</span>
                <span>{{ studSpacingOverrideDisplay }}</span>
              </div>

              <div v-if="wallStudSpacingOverride">
                <div class="CP-summary-row">
                  <span>Stud Spacing:</span>
                  <span>{{ currentStudSpacing }}" O/C</span>
                </div>
              </div>
            </div>
            <!--========================THIS IS THE END OF THE ADVANCED STUD SYSTEM===-->
            <!--==========================DASHBOARD ROOF PEAK DIRECTION===============-->
            <div class="roof-peak-summary-section">
              <div class="CP-summary-row">
                <span>Roof Peak Direction:</span>
                <span>{{ roofPeakDirection }}</span>
              </div>
            </div>
            <!--==================================DASHBOARD ROOF RAFTERS===============-->
            <div class="roof-rafters-summary-section">
              <div class="CP-summary-row">
                <span>Roof Rafters:</span>
                <span>{{ roofRafterSize }}</span>
              </div>
            </div>
            <!--==================================DASHBOARD ROOF PITCH/EAVES/GABLES===========-->
            <div class="roof-pitch-summary-section">
              <div class="CP-summary-row">
                <span>Roof Pitch:</span>
                <span>{{ roofPitch }}:12</span>
              </div>
              <div class="CP-summary-row">
                <span>Eaves Overhang:</span>
                <span>{{ eavesOverHang }}"</span>
              </div>
              <div class="CP-summary-row">
                <span>Gable Ends Overhang, currently n/a:</span>
                <span>{{ gableEndsOverHang }}</span>
              </div>
            </div>
            <!--====================================DASHBOARD FRONT DOOR===============-->
            <!--===========THIS IS THE START OF THE FRONT DOOR SUMMARY SECTION=========-->
            <div class="front-door-summary-section">
              <div class="CP-summary-row">
                <span>Front Door:</span>
                <span>{{ frontDoorDisplay }}</span>
              </div>

              <div v-if="frontDoor === 1">
                <div class="CP-summary-row">
                  <span>Door Width:</span>
                  <span>{{ frontDoorWidth }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Door Height:</span>
                  <span>{{ frontDoorHeight }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Distance From Floor:</span>
                  <span>{{ frontDoorFloorHeight }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Header:</span>
                  <span>{{ frontDoorHeaderSizeDisplay }}</span>
                </div>

                <div class="CP-summary-row">
                  <span>Distance From Left Corner:</span>
                  <span>{{ frontDoorDistanceDisplay }}</span>
                </div>
              </div>
            </div>

            <!--==========================DASHBOARD WINDOWS START HERE===============-->
            <!--===========THIS IS THE START OF THE LEFT WINDOW SUMMARY SECTION=========-->
            <div class="left-window-summary-section">
              <div class="CP-summary-row">
                <span>Left Wall Window:</span>
                <span>{{ leftWallRO2WindowDisplay }}</span>
              </div>
              <div v-if="leftWallRO2Window === 1">
                <div class="CP-summary-row">
                  <span>Window Width:</span>
                  <span>{{ leftWallRO2Width }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Window Height:</span>
                  <span>{{ leftWallRO2Height }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Distance From Floor:</span>
                  <span>{{ leftWallRO2FloorHeight }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Header:</span>
                  <span>{{ leftWallRO2HeaderSizeDisplay }}</span>
                </div>

                <div class="CP-summary-row">
                  <span>Distance From Front Corner:</span>
                  <span>{{ leftWallRO2StudIndexDisplay }}</span>
                </div>
              </div>
            </div>
            <!--============THIS IS THE END OF THE LEFT WINDOW SUMMARY SECTION=========-->
            <!--===========THIS IS THE START OF THE RIGHT WINDOW SUMMARY SECTION=========-->
            <div class="right-window-summary-section">
              <div class="CP-summary-row">
                <span>Right Wall Window:</span>
                <span>{{ rightWallRO4WindowDisplay }}</span>
              </div>
              <div v-if="rightWallRO4Window === 1">
                <div class="CP-summary-row">
                  <span>Window Width:</span>
                  <span>{{ rightWallRO4Width }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Window Height:</span>
                  <span>{{ rightWallRO4Height }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Distance From Floor:</span>
                  <span>{{ rightWallRO4FloorHeight }}"</span>
                </div>

                <div class="CP-summary-row">
                  <span>Header:</span>
                  <span>{{ rightWallRO4HeaderSizeDisplay }}</span>
                </div>

                <div class="CP-summary-row">
                  <span>Distance From Front Corner:</span>
                  <span>{{ rightWallRO4StudIndexDisplay }}</span>
                </div>
              </div>
            </div>
            <button class="load-mechanism-button" @click="loadTheMechanism">
              step 1: confirm your choices, LOAD THE MECHANISM™, save to your
              computer
            </button>
            <div class="next-step-link">
              <a
                href="https://www.beaverbuiltcanada.ca/submit-your-mechanism"
                target="_blank"
                rel="noopener noreferrer"
              >
                step 2: Continue to Submission →
              </a>
            </div>
          </div>
        </aside>
      </div>
      <!--===============================THIS MARKS THE BOTTOM OF THE LHS CONTROL PANEL==
      ==========================AND THE BEGINNING OF THE RHS OPTIONS AND MAIN COLUMN==========
      =================================================================================-->
      <div class="options-column">
        <div class="main-column">
          <!--=======================================DIMENSIONS SECTION===============-->
          <div class="dimensions-section">
            <h2>Building Dimensions</h2>
            <!-- double isometric pic with the view cubes -->

            <div class="traveler-dimensions-Q-card">
              <p>
                Enter the desired dimensions for your building. The floor area,
                perimeter, and wall area will be calculated and displayed below,
                for your reference.
              </p>
              <h4>Dimensions Reference</h4>
              <div class="reference-image-container">
                <img
                  :src="twoIso"
                  alt="Building dimensions reference"
                  class="dimensions_reference_image"
                />
              </div>
              <div>
                <!--Start of RED GREEN BLUE legend-->
                <div class="dimension-legend">
                  <div class="legend-depth">Depth</div>
                  <div class="legend-width">Width</div>
                  <div class="legend-height">Wall Height</div>
                </div>
              </div>

              <!--Start of DEPTH drop down-->
              <div class="dimension-row">
                <label class="dimension-label">Depth:</label>
                <select v-model="depth">
                  <option value="4">4'</option>
                  <option value="5">5'</option>
                  <option value="6">6'</option>
                  <option value="7">7'</option>
                  <option value="8">8'</option>
                  <option value="9">9'</option>
                  <option value="10">10'</option>
                  <option value="11">11'</option>
                  <option value="12">12'</option>
                  <option value="13">13'</option>
                  <option value="14">14'</option>
                  <option value="15">15'</option>
                  <option value="16">16'</option>
                  <option value="17">17'</option>
                  <option value="18">18'</option>
                  <option value="19">19'</option>
                  <option value="20">20'</option>
                  <option value="21">21'</option>
                  <option value="22">22'</option>
                  <option value="23">23'</option>
                  <option value="24">24'</option>
                </select>
                <span class="dimension-value"
                  >Your chosen depth is {{ depth }} feet.</span
                >
                <div v-if="dimensionWarning" class="warning_box">
                  💡 Depth is typically treated as the longer dimension of the
                  building. Future floor joist, center skid, and roof
                  calculations will use this orientation as the default
                  assumption. You may continue with any dimensions, but
                  understanding this convention will help when selecting later
                  options. 🦫
                </div>
              </div>
              <p>
                <button @click="showWhatIsDepth = !showWhatIsDepth">
                  What Is Depth?
                </button>
              </p>
              <div v-if="showWhatIsDepth">
                <p>
                  <strong class="whatIs-q"> Q: What is depth?</strong><br />
                  A: Depth is the front-to-back measurement of the building.
                </p>
                <p>
                  <strong class="whatIs-q"
                    >Q: What do we mean by the building's depth?</strong
                  ><br />
                  A: Imagine you're standing at the front door looking inside.
                  The depth is the distance your line of sight travels from the
                  front wall to the back wall.
                </p>
              </div>
              <!--Start of WIDTH drop down-->
              <div class="dimension-row">
                <label class="dimension-label">Width:</label>
                <select v-model="width">
                  <option value="4">4'</option>
                  <option value="5">5'</option>
                  <option value="6">6'</option>
                  <option value="7">7'</option>
                  <option value="8">8'</option>
                  <option value="9">9'</option>
                  <option value="10">10'</option>
                  <option value="11">11'</option>
                  <option value="12">12'</option>
                  <option value="13">13'</option>
                  <option value="14">14'</option>
                  <option value="15">15'</option>
                  <option value="16">16'</option>
                  <option value="17">17'</option>
                  <option value="18">18'</option>
                  <option value="19">19'</option>
                  <option value="20">20'</option>
                  <option value="21">21'</option>
                  <option value="22">22'</option>
                  <option value="23">23'</option>
                  <option value="24">24'</option>
                </select>
                <span class="dimension-value"
                  >Your chosen width is {{ width }} feet.</span
                >
              </div>
              <p>
                <button @click="showWhatIsWidth = !showWhatIsWidth">
                  What Is Width?
                </button>
              </p>
              <div v-if="showWhatIsWidth">
                <p>
                  <strong class="whatIs-q"> Q: What is width?</strong><br />
                  A: Width is the side-to-side measurement of the building.
                </p>
                <p>
                  <strong class="whatIs-q"
                    >Q: What are we considering to be the width?</strong
                  ><br />
                  A: Imagine you're standing at the front door facing the
                  building. The width runs from left to right across the front
                  wall.
                </p>
              </div>
              <!--===========================================Start of WALL HEIGHT drop down===
        =============================================-->
              <div class="dimension-row">
                <label class="dimension-label">Wall Height:</label>
                <select v-model="wallHeight">
                  <option value="3.5">3 1/2'</option>
                  <option value="4">4'</option>
                  <option value="4.5">4 1/2'</option>
                  <option value="5">5'</option>
                  <option value="5.5">5 1/2'</option>
                  <option value="6">6'</option>
                  <option value="6.5">6 1/2'</option>
                  <option value="7">7'</option>
                  <option value="7.5">7 1/2'</option>
                  <option value="8">8'</option>
                  <option value="8.5">8 1/2'</option>
                  <option value="9">9'</option>
                  <option value="9.5">9 1/2'</option>
                  <option value="10">10'</option>
                  <option value="10.5">10 1/2'</option>
                  <option value="11">11'</option>
                  <option value="11.5">11 1/2'</option>
                  <option value="12">12'</option>
                </select>
                <span class="dimension-value"
                  >Your chosen wall height is {{ wallHeight }} feet.</span
                >
              </div>
              <p>
                <button @click="showWhatIsWallHeight = !showWhatIsWallHeight">
                  What Is Wall Height?
                </button>
              </p>
              <div v-if="showWhatIsWallHeight">
                <p>
                  <strong class="whatIs-q"> Q: What is wall height?</strong
                  ><br />
                  A: Wall height is the vertical distance from the floor to the
                  top of the building's walls.
                </p>

                <p>
                  <strong class="whatIs-q"
                    >Q: How do we determine the wall height?</strong
                  ><br />
                  A: Wall height is measured from the top of the floor to the
                  top of the wall where the roof structure begins. It does not
                  include the gable, which is the triangular section of wall
                  above the main wall on a gable roof.
                </p>
              </div>

              <p>reference, Floor Area: {{ floorArea }} sq ft</p>
              <p>reference, Perimeter: {{ perimeter }} ft</p>
              <p>reference, Wall Area: {{ wallArea }} sq ft</p>
            </div>
          </div>
          <!--=================================START OF FOUNDATION SECTION==================-->
          <!--======================================================================-->
          <div class="foundation-section">
            <h2>Foundation Skids</h2>

            <div class="travelerQ-card reference_images">
              <h4>Foundation Skids Reference</h4>
              <div class="reference-image-container">
                <img
                  :src="skidsShowing"
                  alt="Foundation skids reference"
                  class="skids_reference_image"
                />
              </div>
              <p>
                <button @click="showWhatIsASkid = !showWhatIsASkid">
                  What Is A Skid?
                </button>
              </p>

              <div v-if="showWhatIsASkid">
                <p>
                  <strong class="whatIs-q"> Q: What is a skid?</strong><br />
                  A: A skid is a horizontal support beam that runs along the
                  bottom of a structure, providing stability and support. Skids
                  are typically made of pressure-treated lumber because they are
                  usually in contact with the ground. They are used in various
                  types of buildings, including sheds, workshops, and animal
                  housing. They help distribute the weight of the structure
                  evenly and can also make it easier to move the building if
                  necessary. By not being permanently fixed to the ground, skids
                  allow for some flexibility in placement and can help prevent
                  moisture damage by elevating the structure slightly above the
                  ground level. Skids also do not require permanent anchoring,
                  which can sometimes be an advantage when navigating building
                  codes and municipal bylaws. As always, be sure to follow the
                  rules and requirements of your local jurisdiction.
                </p>
                <p>
                  By default, this configurator will use four skids around the
                  underside perimeter of the building. For larger depth span
                  buildings it is recommended to use a center skid also. The
                  center skid will be automatically positioned for equal spacing
                  and parallel between the front skid and the back skid.
                </p>
              </div>
            </div>
            <div
              class="travelerQ-card"
              :class="{ selected_wood004: skidType === '2x4flat' }"
              @click="skidType = '2x4flat'"
            >
              <h3>Use 2x4 Pressure Treated - Flat Orientation</h3>
              <p>
                Recommended for smaller structures like pets and small animal
                housing.
              </p>
            </div>

            <div v-if="skidType" class="current_selection">
              <p>
                Selected Foundation Skid Type; Pressure Treated: {{ skidType }}
              </p>
            </div>

            <div
              class="travelerQ-card"
              :class="{ selected_wood004: skidType === '4x4' }"
              @click="skidType = '4x4'"
            >
              <h3>Use 4x4 Pressure Treated</h3>
              <p>
                Recommended for larger size structures like large animal
                housing, sheds and small buildings.
              </p>
            </div>

            <div
              class="travelerQ-card"
              :class="{
                selected_wood004: centerSkid,
                current_selection: centerSkid,
              }"
              @click="centerSkid = centerSkid ? '' : 'Use Center Skid'"
            >
              <h3>Use a Center Skid</h3>
              <p>
                Recommended for larger size structures with a deep span and
                heavier floor loads. Such as large equipment sheds and large
                buildings.
              </p>
            </div>

            <div class="current_selection" v-if="centerSkid">
              Selected Center Skid: {{ centerSkid }}
            </div>
          </div>
          <!--=====================================================FLOOR JOISTS SECTION======
=====================================================================================-->
          <div class="floor-joist-section">
            <h2>Floor Joists</h2>
            <p>
              <button @click="showWhatIsAJoist = !showWhatIsAJoist">
                What is a floor joist?
              </button>
            </p>
            <div v-if="showWhatIsAJoist">
              <p>
                <strong class="whatIs-q"> Q: What is a floor joist?</strong
                ><br />
                A: A floor joist is a horizontal structural element that
                supports the floor of a building. They are typically made of
                wood or steel and are used to distribute the weight of the floor
                evenly across the structure.
              </p>

              <p>
                By default, this configurator will place the floor joists in a
                depth orientation. If you are designing a rectangular shaped
                structure, it would be better to have the depth be the longer
                side, in the event you choose to use a center skid to support
                the floor joists in the middle of their length.
              </p>
            </div>

            <div
              class="traveler-2x4stud-Q-card"
              :class="{ selected_wood034: floorJoistSize === '2x4' }"
              @click="floorJoistSize = '2x4'"
            >
              <h3>Use 2x4 Floor Joists</h3>
              <p>
                <button @click="show2x4FloorJoistInfo = !show2x4FloorJoistInfo">
                  more about 2x4 joists
                </button>
              </p>

              <div v-if="show2x4FloorJoistInfo">
                <p>
                  2x4 joists are recommended for smaller structures like pets
                  and smaller animal housing, and small light duty sheds for
                  storage, gardening tools and push type lawnmowers.
                </p>
                Conventional framing practice is to use the same size floor
                joists as used for wall studs and roof rafters. However, you may
                choose an unconventional approach by mixing sizes, to suit your
                design intent.

                <p>
                  Using 2x4 joists defaults to conventional framing method of
                  16" on centers. You may choose to over-ride that by using more
                  advanced settings later. (floor joist spacing over-ride is
                  currently under development)
                </p>
              </div>
            </div>

            <div class="current_selection" v-if="floorJoistSize">
              Selected Floor Joist Size: {{ floorJoistSize }}
            </div>

            <div
              class="traveler-2x6stud-Q-card"
              :class="{ selected_wood034: floorJoistSize === '2x6' }"
              @click="floorJoistSize = '2x6'"
            >
              <h3>Use 2x6 Floor Joists</h3>
              <p>
                <button @click="show2x6FloorJoistInfo = !show2x6FloorJoistInfo">
                  more about 2x6 joists
                </button>
              </p>
              <div v-if="show2x6FloorJoistInfo">
                <p>
                  2x6 joists are recommended for larger size structures like
                  equipment sheds and small buildings.
                </p>
                <p>
                  Conventional framing practice is to use the same size floor
                  joists as used for wall studs and roof rafters. However, you
                  may choose an unconventional approach by mixing sizes, to suit
                  your design intent.
                </p>
                <p>
                  Using 2x6 floor joists defaults to conventional framing method
                  of 24" on centers. You may choose to over-ride that by using
                  more advanced settings later.(floor joist spacing over-ride is
                  currently under development)
                </p>
              </div>
            </div>
          </div>
          <!--=========================================================PLYWOOD SECTION=====-->
          <div class="plywood-section">
            <h2>Floor Plywood</h2>
            <div class="travelerQ-card selected_osb">
              <label class="dimension-label">Plywood Thickness: </label>
              <select v-model="floorPlywoodThickness">
                <option value="1">1/2"</option>
                <option value="2">5/8"</option>
                <option value="3">3/4"</option>
                <option value="4">1"</option>
              </select>
            </div>
          </div>
          <!--===============================THIS IS THE START OF WALL STUDS SECTION=======
          ======================================================================-->
          <div class="stud-section">
            <h2>Wall Studs</h2>
            <p>
              <button @click="showWhatIsAStud = !showWhatIsAStud">
                What is a wall stud?
              </button>
            </p>
            <div v-if="showWhatIsAStud">
              <strong class="whatIs-q"> Q: What is a wall stud?</strong><br />
              A: A wall stud is a vertical structural element that supports the
              walls of a building. They are typically made of wood or steel and
              are used to distribute the weight of the structure evenly across
              the walls.
            </div>

            <div
              class="traveler-2x4stud-Q-card"
              :class="{ selected_wood034: wallStudSize === '2x4' }"
              @click="wallStudSize = '2x4'"
            >
              <h3>Use 2x4 Wall Studs</h3>
              <p>
                <button @click="showMoreAbout2x4Studs = !showMoreAbout2x4Studs">
                  more about 2x4 studs
                </button>
              </p>

              <div v-if="showMoreAbout2x4Studs">
                <p>
                  2x4 studs are recommended for smaller structures like animal
                  housing, and small light duty sheds for storage, gardening
                  tools and push type lawnmowers.
                </p>
                Conventional framing practice is to use the same size studs as
                used for floor joists and roof rafters. However, you may choose
                an unconventional approach by mixing sizes.

                <p>
                  Using 2x4 studs defaults to conventional 16" on centers. You
                  may choose to over-ride that by using more advanced settings
                  later.
                </p>
              </div>
            </div>
            <div class="current_selection" v-if="wallStudSize">
              Selected Wall Stud Size: {{ wallStudSize }}
            </div>

            <div
              class="traveler-2x6stud-Q-card"
              :class="{ selected_wood034: wallStudSize === '2x6' }"
              @click="wallStudSize = '2x6'"
            >
              <h3>Use 2x6 Wall Studs</h3>
              <p>
                <button @click="showMoreAbout2x6Studs = !showMoreAbout2x6Studs">
                  more about 2x6 studs
                </button>
              </p>
              <div v-if="showMoreAbout2x6Studs">
                <p>
                  2x6 studs are recommended for larger structures like equipment
                  sheds and small buildings.
                </p>
                <p>
                  Conventional framing practice is to use the same size studs as
                  used for floor joists and roof rafters. However, you may
                  choose an unconventional approach by mixing sizes.
                </p>
                <p>
                  Using 2x6 wall studs defaults to conventional 24" on centers.
                  You may choose to over-ride that by using more advanced
                  settings below.
                </p>
              </div>
            </div>
            <p>
              <button
                @click="showAdvancedStudOptions = !showAdvancedStudOptions"
              >
                🪵 Advanced Stud Spacing Options
              </button>
            </p>

            <div v-if="showAdvancedStudOptions" class="traveler-studQ-card">
              <h3>Stud Spacing Override</h3>

              <!-- Override toggle goes here -->
              <p>
                <label>
                  <input type="checkbox" v-model="wallStudSpacingOverride" />
                  Override Default Stud Spacing
                </label>
              </p>
              <div class="stud-spacing-row" v-if="wallStudSpacingOverride">
                <label>Custom Stud Spacing (12" – 48")</label>

                <input
                  type="number"
                  v-model="wallStudOverrideSpacing"
                  min="12"
                  max="48"
                  step="2"
                />
                inches
              </div>

              <!-- Manual spacing input goes here -->
            </div>
          </div>
          <!--===========================================ROOF PEAK DIRECTION=========
          ==========================================================================-->
          <div class="roof-peak-section">
            <h2>Roof Peak Direction</h2>

            <p>
              <button @click="showMoreAboutRoofPeak = !showMoreAboutRoofPeak">
                more about roof peaks and gables
              </button>
            </p>
            <div v-if="showMoreAboutRoofPeak">
              <p>
                <strong class="whatIs-q">Q: What is a roof peak?</strong><br />
                A: A roof peak is the highest point of a roof, typically found
                at the center of a gable roof.
              </p>

              <p>
                <strong class="whatIs-q">Q: What is a gable?</strong><br />
                A: A gable is the triangular section of a wall between the edges
                of a sloping roof.
              </p>
            </div>

            <div
              class="travelerQ-card roof_option"
              :class="{ selected_roofPeak: roofPeakDirection === 'width' }"
              @click="roofPeakDirection = 'width'"
            >
              <img
                :src="roofPeakWidth"
                alt="Roof peak runs width"
                class="roof_direction_image"
              />
              <h3>Peak Runs Width</h3>
              <p>Gable ends on the left and right walls.</p>
              <p>Eaves overhangs on the front and back.</p>
            </div>

            <p class="current_selection" v-if="roofPeakDirection">
              Selected Roof Direction: {{ roofPeakDirection }}
            </p>

            <div
              class="travelerQ-card roof_option"
              :class="{ selected_roofPeak: roofPeakDirection === 'depth' }"
              @click="roofPeakDirection = 'depth'"
            >
              <img
                :src="roofPeakDepth"
                alt="Roof peak runs depth"
                class="roof_direction_image"
              />
              <h3>Peak Runs Depth</h3>
              <p>Gable ends on the front and rear walls.</p>
              <p>Eaves overhangs on the left and right.</p>
            </div>
          </div>
          <div class="roof-rafters-section">
            <h2>Roof Rafters</h2>
            <p>
              <button @click="showWhatIsARoofRafter = !showWhatIsARoofRafter">
                What is a roof rafter?
              </button>
            </p>
            <p v-if="showWhatIsARoofRafter">
              <strong class="whatIs-q"> Q: What is a roof rafter?</strong><br />
              A: A roof rafter is a structural component that supports the roof
              and transfers the load to the walls.
            </p>
            <div class="current_selection" v-if="roofRafterSizeSize">
              Selected Roof Rafter Size: {{ roofRafterSize }}
            </div>

            <div
              class="travelerQ-card"
              :class="{ selected_wood034: roofRafterSize === '2x4' }"
              @click="roofRafterSize = '2x4'"
            >
              <h3>Use 2x4 Roof Rafters</h3>
              <p>
                <button
                  @click="
                    showMoreAbout2x4RoofRafters = !showMoreAbout2x4RoofRafters
                  "
                >
                  More about 2x4 roof rafters
                </button>
              </p>
              <div v-if="showMoreAbout2x4RoofRafters">
                <p>
                  2x4 roof rafters are typically used in smaller structures and
                  are less expensive than larger rafters.
                </p>
                <p>
                  2x4 roof rafters are recommended for smaller structures like
                  animal housing, and small light duty sheds for storage,
                  gardening tools and push type lawnmowers.
                </p>

                <p>
                  Conventional framing practice is to use the same size roof
                  rafters as used for floor joists and wall studs. However, you
                  may choose an unconventional approach by mixing sizes.
                </p>
                <p>
                  Using 2x4 roof rafters defaults to conventional 16" on
                  centers. You may choose to over-ride that by using more
                  advanced settings later.(This feature is currently in
                  development.)
                </p>
              </div>
            </div>

            <div class="current_selection" v-if="roofRafterSize">
              Selected Roof Rafter Size: {{ roofRafterSize }}
            </div>

            <div
              class="travelerQ-card"
              :class="{ selected_wood034: roofRafterSize === '2x6' }"
              @click="roofRafterSize = '2x6'"
            >
              <h3>Use 2x6 Roof Rafters</h3>
              <p>
                <button
                  @click="
                    showMoreAbout2x6RoofRafters = !showMoreAbout2x6RoofRafters
                  "
                >
                  More about 2x6 roof rafters
                </button>
              </p>
              <div v-if="showMoreAbout2x6RoofRafters">
                <p>
                  2x6 roof rafters are recommended for larger structures such as
                  equipment sheds, workshops, garages, and small buildings.
                </p>

                <p>
                  Compared to 2x4 rafters, 2x6 rafters provide greater strength
                  and are better suited for longer spans and heavier roof loads.
                </p>

                <p>
                  Using 2x6 roof rafters defaults to conventional 24" on-center
                  spacing. This can be customized later using advanced settings.
                  (This feature is currently in development.)
                </p>
              </div>
            </div>
          </div>
          <!--================================================ROOF PITCH SECTION==========-->
          <div class="roof-pitch-section">
            <h2>Roof Pitch</h2>
            <p>
              <button @click="showWhatIsRoofPitch = !showWhatIsRoofPitch">
                More about roof pitch
              </button>
            </p>
            <div v-if="showWhatIsRoofPitch">
              <p>
                <strong class="whatIs-q"> Q: What is roof pitch?</strong><br />
                A: Roof pitch is the slope of the roof, typically expressed as a
                ratio of rise over run.
              </p>
              <p>
                <strong class="whatIs-q"> Q: What is rise?</strong><br />
                A: Rise is the vertical distance the roof rises over a
                horizontal distance (run).
              </p>
              <p>
                The smaller the ratio, the flatter the roof. A 3:12 ratio is 15
                degrees, a 6:12 ratio is 26.57 degrees, and a 12:12 ratio is 45
                degrees.
              </p>
            </div>
            <div class="travelerQ-card">
              <label class="dimension-label">Roof Pitch: </label>
              <input type="number" v-model="roofPitch" min="3" max="12" />
              <span class="dimension-value">:12</span>
            </div>

            <h2>Roof Over Hang: Eaves</h2>
            <p>
              <button @click="showWhatAreEaves = !showWhatAreEaves">
                What are eaves?
              </button>
            </p>
            <div v-if="showWhatAreEaves">
              <p>
                <strong class="whatIs-q"> Q: What are eaves?</strong><br />
                A: Eaves are the portions of the roof that extend past the
                exterior walls of the building. They help direct rain and snow
                away from the siding, doors, windows and walls and can provide
                additional shade and weather protection.
              </p>
            </div>
            <div class="travelerQ-card">
              <label class="dimension-label">Eaves Over Hang: </label>
              <input
                type="number"
                v-model="eavesOverHang"
                min="2"
                max="20"
                step="2"
              />
              <span class="dimension-value">inches</span>
            </div>

            <h2>Roof Over Hang: Gable Ends</h2>
            <p>
              <button @click="showWhatAreGableEnds = !showWhatAreGableEnds">
                What are gable ends?
              </button>
            </p>
            <div v-if="showWhatAreGableEnds">
              <p>
                <strong class="whatIs-q">
                  Q: What are gable ends overhangs?</strong
                ><br />
                A: Gable overhangs are the parts of the roof that extend beyond
                the gable-end walls of the building. They provide additional
                weather protection and can improve the overall appearance of the
                structure.
              </p>
            </div>
            <div class="travelerQ-card">
              <label class="dimension-label">Gable Ends Over Hang: </label>
              <input
                type="number"
                v-model="gableEndsOverHang"
                min="0"
                max="0"
                step="2"
              />
              <span class="dimension-value">inches</span>
              <p>
                We are currently working on gable ends overhangs and hope to
                have that option available soon.
              </p>
            </div>
          </div>
          <!--===================================================================
          =================================FRONT DOOR SECTION===================
          ========================================================================-->
          <div class="door-section-container">
            <div class="door-section">
              <h2>Front Door</h2>

              <p>
                <button @click="showWhatIsAFrontDoor = !showWhatIsAFrontDoor">
                  What is a front door?
                </button>
              </p>

              <p v-if="showWhatIsAFrontDoor">
                <strong class="whatIs-q">Q: What is a front door?</strong><br />
                A: A front door is an optional rough opening placed in the front
                wall of the structure. Door dimensions, height above the floor,
                and framing options can be customized. It could potentially be
                used as a door, a window, or a higher positioned chicken coop
                door.
              </p>

              <div class="current_selection" v-if="frontDoor === 1">
                Front Door: Included
              </div>

              <div
                class="traveler-frontDoor-Q-card"
                :class="{ front_selected_door: frontDoor === 1 }"
                @click="frontDoor = frontDoor === 1 ? 0 : 1"
              >
                <h3>🚪 Add Front Door</h3>
              </div>

              <div v-if="frontDoor === 1">
                <button
                  @click="showMoreAboutFrontDoor = !showMoreAboutFrontDoor"
                >
                  {{
                    showMoreAboutFrontDoor
                      ? "▼ Advanced Front Door Options"
                      : "▶ Advanced Front Door Options"
                  }}
                </button>

                <div v-if="showMoreAboutFrontDoor">
                  <!--============================Start Of Advanced Door Options==============
        ================================================================================-->

                  <h2>🚪 Advanced Front Door Options</h2>

                  <div class="traveler-frontDoor-Q-card">
                    <p>
                      These options control the size and placement of the front
                      wall rough opening. The reference image below is intended
                      to help visualize how each setting affects the final
                      design.
                    </p>
                    <p class="tip-inline">
                      💡 Tip: You can make this opening a higher positioned door
                      opening, for example a chicken coop door, or make it a
                      window opening, simply by changing the default 0 distance
                      from floor.
                    </p>

                    <h4>Front Door Reference Diagram</h4>

                    <!--=====================================================DOOR LEGEND=========
============================================================================-->

                    <div class="left-window-dimension-legend">
                      <div class="window-legend-width">Door Width</div>
                      <div class="window-legend-height">Door Height</div>
                      <div class="window-legend-headers">Headers</div>
                      <div class="window-legend-stud-index">
                        Distance From Left Corner (stud index 1)
                      </div>
                      <div class="window-legend-dff">Dist From Floor</div>
                    </div>

                    <div class="reference-image-container">
                      <img
                        :src="frontDoorReferenceImage"
                        alt="Front door reference"
                        class="dimensions_reference_image"
                      />
                    </div>

                    <h4>Door Reference</h4>

                    <ul>
                      <li>
                        For added clarity, this reference image illustrates a
                        typical 12' wide × 12' deep × 8' wall height structure
                        with the roof peak running in the depth direction.
                      </li>

                      <li>
                        The front door rough opening shown is representative
                        only, and is 36" wide × 82" height. Your actual
                        requested dimensions and location may differ based on
                        your selections.
                      </li>

                      <li>
                        The door is located approximately 16" from the left
                        corner. This means there is 1 empty bay. Your actual
                        requested measurement and the drawings may differ
                        slightly, as the model will attempt to place the rough
                        opening at the closest available stud.
                      </li>

                      <li>
                        A Distance From Left Corner of "Centered" represents the
                        default centered position in the wall.
                      </li>

                      <li>
                        This image is provided as a reference to help explain
                        door configuration settings. Your actual building
                        dimensions, opening sizes, and locations may differ
                        based on the selections made within the configurator.
                      </li>

                      <li>
                        The reference images throughout the configurator are
                        also representative of the style and level of detail
                        customers can expect to see in their final drawing
                        package. Additional drawing examples and sample plans
                        will be provided as the project continues to develop.
                      </li>

                      <p>
                        💡 Tip: Check with your window and door supplier for
                        their recommended rough opening sizes for your
                        particular products.
                      </p>
                      <p class="tip-inline">
                        💡 Tip: You can use the arrows or directly type your
                        input values. Quarter-inch increments are supported.
                      </p>
                    </ul>

                    <!-- DOOR WIDTH -->

                    <div class="dimension-row">
                      <label class="window-legend-width dimension-label">
                        Door Width:
                      </label>

                      <input
                        type="number"
                        v-model="frontDoorWidth"
                        min="12"
                        max="108"
                        step="0.25"
                      />

                      <div v-if="frontDoorWidthWarning" class="warning_box">
                        ⚠️ The selected opening width leaves less than 10" of
                        available wall space, which may be insufficient room for
                        a {{ frontDoorWidth }}" opening. The Fusion model can
                        safely resolve this condition, however the final design
                        may require adjustments during human review. 🦫
                      </div>

                      <span class="window-legend-width dimension-value">
                        {{ frontDoorWidth }}"
                      </span>
                    </div>

                    <!-- DOOR HEIGHT -->

                    <div class="dimension-row">
                      <label class="window-legend-height dimension-label">
                        Door Height:
                      </label>

                      <input
                        type="number"
                        v-model="frontDoorHeight"
                        min="12"
                        max="96"
                        step="0.25"
                      />

                      <div v-if="frontDoorFitWarning" class="warning_box">
                        ⚠️ The selected rough opening height, distance from
                        floor, and header size may exceed the available wall
                        height. The Fusion model includes safeguards and may
                        adjust the design, however final placement may require
                        review.
                      </div>

                      <span class="window-legend-height dimension-value">
                        {{ frontDoorHeight }}"
                      </span>
                    </div>

                    <!-- DISTANCE FROM FLOOR -->

                    <div class="dimension-row">
                      <label class="window-legend-dff dimension-label">
                        Distance From Floor:
                      </label>

                      <input
                        type="number"
                        v-model="frontDoorFloorHeight"
                        min="0"
                        step="0.25"
                      />

                      <span class="window-legend-dff dimension-value">
                        {{ frontDoorFloorHeight }}"
                      </span>
                    </div>

                    <!-- HEADER SIZE -->

                    <div class="dimension-row">
                      <label class="window-legend-headers dimension-label">
                        Header Size:
                      </label>

                      <select v-model="frontDoorHeaderSize">
                        <option value="2x4">2x4</option>
                        <option value="2x6">2x6</option>
                      </select>

                      <span class="window-legend-headers dimension-value">
                        {{ frontDoorHeaderSize }}
                      </span>
                    </div>

                    <!-- DISTANCE FROM LEFT CORNER -->

                    <div class="dimension-row">
                      <label class="window-legend-stud-index dimension-label">
                        Distance From Left Corner:
                      </label>

                      <input
                        type="number"
                        v-model="frontDoorStudIndex"
                        min="0"
                        :step="currentStudSpacing"
                      />

                      <span class="window-legend-stud-index dimension-value">
                        {{ frontDoorDistanceDisplay }}
                      </span>

                      <div v-if="frontDoorWarning" class="warning_box">
                        ⚠️ Distance From Left Corner leaves less than 12" of
                        clearance from the right corner, which may be
                        insufficient room for a
                        {{ frontDoorWidth }}" opening. The Fusion model can
                        safely resolve this condition, however the final design
                        may require adjustments during human review. 🦫
                      </div>
                    </div>

                    <p>
                      <button
                        @click="showMoreAboutFrontDoor = false"
                        class="clear-window-button"
                      >
                        🚪 Close Front Door Options
                      </button>
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!--CLOSING DIV ABOVE THIS LINE IS THE DIV THAT CLOSES THE FRONT DOOR==
          ========================================================================-->

          <!--===================================================================
          =========================START OF LEFT WALL WINDOW - at top header======================
          ========================================================================-->
          <div class="window-section-container">
            <div class="window-section">
              <h2>Left Wall Window</h2>

              <p>
                <button
                  @click="
                    showWhatIsALeftWallWindow = !showWhatIsALeftWallWindow
                  "
                >
                  What is a left wall window?
                </button>
              </p>

              <p v-if="showWhatIsALeftWallWindow">
                <strong class="whatIs-q">Q: What is a left wall window?</strong
                ><br />
                A: A left wall window is an optional rough opening placed in the
                left wall of the structure. Window dimensions, height above the
                floor, and framing options can be customized.
              </p>

              <div class="current_selection" v-if="leftWallRO2Window === 1">
                Left Wall Window: Included
              </div>

              <div
                class="left-traveler-windowQ-card"
                :class="{ left_selected_window: leftWallRO2Window === 1 }"
                @click="leftWallRO2Window = leftWallRO2Window === 1 ? 0 : 1"
              >
                <h3>🪟 Add Left Wall Window</h3>
              </div>

              <div v-if="leftWallRO2Window === 1">
                <button
                  @click="
                    showMoreAboutLeftWallWindow = !showMoreAboutLeftWallWindow
                  "
                >
                  {{
                    showMoreAboutLeftWallWindow
                      ? "▼ Advanced Left Window Options"
                      : "▶ Advanced Left Window Options"
                  }}
                </button>

                <div v-if="showMoreAboutLeftWallWindow">
                  <!--============================Start Of Advanced Window Options==============
              ================================================================================-->
                  <h2>🪟 Advanced Left Window Options</h2>

                  <div class="left-traveler-windowQ-card">
                    <p>
                      These options control the size and placement of the left
                      wall rough opening. The reference image below is intended
                      to help visualize how each setting affects the final
                      design.
                    </p>
                    <p>
                      💡 Tip: You can make this a door opening, simply by
                      changing the distance from floor to 0 (zero).
                    </p>

                    <h4>Left Window Reference Diagram</h4>
                    <!--=====================================================WINDOW LEGEND=========
============================================================================-->
                    <div class="left-window-dimension-legend">
                      <div class="window-legend-width">RO Width</div>
                      <div class="window-legend-height">RO Height</div>
                      <div class="window-legend-headers">Headers</div>
                      <div class="window-legend-stud-index">
                        Distance From Front Corner (stud index 1)
                      </div>
                      <div class="window-legend-dff">Dist From Floor</div>
                    </div>

                    <div class="reference-image-container">
                      <img
                        src="./assets/Left-Window-Reference-Image_v1.png"
                        alt="Window reference"
                        class="dimensions_reference_image"
                      />
                    </div>
                    <h4>Window Reference</h4>
                    <ul>
                      <li>
                        For added clarity, this reference image illustrates a
                        typical 8' wide × 12' deep × 8' wall height structure
                        with the roof peak running in the depth direction.
                      </li>
                      <li>
                        The left wall window rough opening shown is 42" wide ×
                        32" high, with the rough opening positioned 36" above
                        the floor.
                      </li>
                      <li>
                        The window is located aproximately 16" from the front
                        corner. This means there is 1 empty bay. Your actual
                        requested measurement and the drawings may differ
                        slightly, as the model will attempt to place the rough
                        opening at the closest available stud.
                      </li>
                      <li>
                        A Distance From Front Corner of "Centered" represents
                        the default centered position in the wall.
                      </li>
                      <li>
                        This image is provided as a reference to help explain
                        window configuration settings. Your actual building
                        dimensions, window sizes, and locations may differ based
                        on the selections made within the configurator.
                      </li>
                      <li>
                        The reference images throughout the configurator are
                        also representative of the style and level of detail
                        customers can expect to see in their final drawing
                        package. Additional drawing examples and sample plans
                        will be provided as the project continues to develop.
                      </li>
                      <p class="tip-inline">
                        💡 Tip: You can use the arrows or directly type your
                        input values. Quarter-inch increments are supported.
                      </p>
                      <p>
                        💡 Tip: Check with your window and door supplier for
                        their recommended rough opening sizes for your
                        particular windows and doors.
                      </p>
                    </ul>

                    <!-- RO WIDTH -->

                    <div class="dimension-row">
                      <label class="window-legend-width dimension-label"
                        >RO Width:</label
                      >

                      <input
                        type="number"
                        v-model="leftWallRO2Width"
                        min="12"
                        max="72"
                        step="0.25"
                      />
                      <div v-if="leftWindowWidthWarning" class="warning_box">
                        ⚠️ The selected opening width leaves less than 10" of
                        available wall space, which may be insufficient room for
                        a {{ leftWallRO2Width }}" opening. The Fusion model can
                        safely resolve this condition, however the final design
                        may require adjustments during human review. 🦫
                      </div>

                      <span class="window-legend-width dimension-value">
                        {{ leftWallRO2Width }}"
                      </span>
                    </div>

                    <!-- RO HEIGHT -->

                    <div class="dimension-row">
                      <label class="window-legend-height dimension-label"
                        >RO Height:</label
                      >

                      <input
                        type="number"
                        v-model="leftWallRO2Height"
                        min="12"
                        max="72"
                        step="0.25"
                      />
                      <div v-if="leftWindowHeightWarning" class="warning_box">
                        ⚠️ The selected rough opening height, distance from
                        floor, and header size may exceed the available wall
                        height. The Fusion model includes safeguards and may
                        adjust the design, however final placement may require
                        review.
                      </div>

                      <span class="window-legend-height dimension-value">
                        {{ leftWallRO2Height }}"
                      </span>
                    </div>

                    <!-- DISTANCE FROM FLOOR -->

                    <div class="dimension-row">
                      <label class="window-legend-dff dimension-label"
                        >Distance From Floor:</label
                      >

                      <input
                        type="number"
                        v-model="leftWallRO2FloorHeight"
                        min="0"
                        max="72"
                        step="0.25"
                      />

                      <span class="window-legend-dff dimension-value">
                        {{ leftWallRO2FloorHeight }}"
                      </span>
                    </div>

                    <!-- HEADER SIZE -->

                    <div class="dimension-row">
                      <label class="window-legend-headers dimension-label"
                        >Header Size:</label
                      >

                      <select v-model="leftWallRO2HeaderSize">
                        <option value="2x4">2x4</option>
                        <option value="2x6">2x6</option>
                      </select>
                      <span class="window-legend-headers dimension-value">
                        {{ leftWallRO2HeaderSize }}
                      </span>
                    </div>

                    <!-- STUD INDEX -->

                    <div class="dimension-row">
                      <label class="window-legend-stud-index dimension-label"
                        >Distance From Front Corner:</label
                      >

                      <input
                        type="number"
                        v-model="leftWallRO2StudIndex"
                        min="0"
                        :step="currentStudSpacing"
                      />

                      <span class="window-legend-stud-index dimension-value">
                        {{ leftWallRO2StudIndexDisplay }}
                      </span>
                      <div v-if="leftWindowFitWarning" class="warning_box">
                        ⚠️ Distance From Front Corner leaves less than 12" of
                        clearance from the back corner, which may be
                        insufficient room for a
                        {{ leftWallRO2Width }}" opening. The Fusion model can
                        safely resolve this condition, however the final design
                        may require adjustments during human review. 🦫
                      </div>
                    </div>
                    <p>
                      <button
                        @click="showMoreAboutLeftWallWindow = false"
                        class="clear-window-button"
                      >
                        🪟 Close Left Window Options
                      </button>
                    </p>
                  </div>
                </div>
              </div>
            </div>
            <!--CLOSING DIV ABOVE THIS LINE IS THE DIV THAT CLOSES THE LEFT WINDOW==
          =========================START OF RIGHT WALL WINDOW======================
          ========================================================================-->

            <div class="window-section">
              <h2>Right Wall Window</h2>

              <p>
                <button
                  @click="
                    showWhatIsARightWallWindow = !showWhatIsARightWallWindow
                  "
                >
                  What is a right wall window?
                </button>
              </p>

              <p v-if="showWhatIsARightWallWindow">
                <strong class="whatIs-q">Q: What is a right wall window?</strong
                ><br />
                A: A right wall window is an optional rough opening placed in
                the right wall of the structure. Window dimensions, height above
                the floor, and framing options can be customized.
              </p>

              <div class="current_selection" v-if="rightWallRO4Window === 1">
                Right Wall Window: Included
              </div>

              <div
                class="right-traveler-windowQ-card"
                :class="{ right_selected_window: rightWallRO4Window === 1 }"
                @click="rightWallRO4Window = rightWallRO4Window === 1 ? 0 : 1"
              >
                <h3>🪟 Add Right Wall Window</h3>
              </div>

              <div v-if="rightWallRO4Window === 1">
                <button
                  @click="
                    showMoreAboutRightWallWindow = !showMoreAboutRightWallWindow
                  "
                >
                  {{
                    showMoreAboutRightWallWindow
                      ? "▼ Advanced Right Window Options"
                      : "▶ Advanced Right Window Options"
                  }}
                </button>

                <div v-if="showMoreAboutRightWallWindow">
                  <!--============================Start Of Advanced Window Options==============
              ================================================================================-->
                  <h2>🪟 Advanced Right Window Options</h2>

                  <div class="right-traveler-windowQ-card">
                    <p>
                      These options control the size and placement of the right
                      wall rough opening. The reference image below is intended
                      to help visualize how each setting affects the final
                      design.
                    </p>
                    <p>
                      💡 Tip: You can make this a door opening, simply by
                      changing the distance from floor to 0 (zero).
                    </p>

                    <h4>Right Window Reference Diagram</h4>
                    <!--===================================WINDOW LEGEND=========
============================================================================-->
                    <div class="right-window-dimension-legend">
                      <div class="window-legend-width">RO Width</div>
                      <div class="window-legend-height">RO Height</div>
                      <div class="window-legend-headers">Headers</div>
                      <div class="window-legend-stud-index">
                        Distance From Front Corner (Fourth Bay Stud Index)
                      </div>
                      <div class="window-legend-dff">Dist From Floor</div>
                    </div>

                    <div class="reference-image-container">
                      <img
                        src="./assets/Right-Window-Reference-Image_v1.png"
                        alt="Right Window reference"
                        class="dimensions_reference_image"
                      />
                    </div>
                    <h4>Window Reference</h4>
                    <ul>
                      <!--==========USE A DIFFERENT SIZE AND ROOF DIRECTION THAN LHS=========-->
                      <li>
                        For added clarity, this reference image illustrates a
                        typical 10' wide × 10' deep × 8' wall height structure
                        with the roof peak running in the width direction and
                        the door rough opening is 60"w × 80"h.
                      </li>
                      <li>
                        The right wall window rough opening shown is 36" wide ×
                        42" high, with the rough opening positioned 28" above
                        the floor.
                      </li>
                      <li>
                        The window is located at approx 64" Distance From Front
                        Corner (Stud Index 4). This means there are 4 empty
                        bays. The greater Distance From Front Corner, the
                        farther the opening is positioned from the front corner.
                      </li>
                      <li>
                        A Distance From Front Corner of "Centered" represents
                        the default centered position in the wall. The greater
                        Distance From Front Corner, the farther the opening is
                        positioned from the front corner.
                      </li>
                      <li>
                        This image is provided as a reference to help explain
                        window configuration settings. Your actual building
                        dimensions, window sizes, and locations may differ based
                        on the selections made within the configurator.
                      </li>
                      <li>
                        The reference images throughout the configurator are
                        also representative of the style and level of detail
                        customers can expect to see in their final drawing
                        package. Additional drawing examples and sample plans
                        will be provided as the project continues to develop.
                      </li>
                      <p class="tip-inline">
                        💡 Tip: You can use the arrows or directly type your
                        input values. Quarter-inch increments are supported.
                      </p>
                      <p>
                        💡 Tip: Check with your window and door supplier for
                        their recommended rough opening sizes for your
                        particular windows and doors.
                      </p>
                    </ul>

                    <!-- RO WIDTH -->

                    <div class="dimension-row">
                      <label class="window-legend-width dimension-label"
                        >RO Width:</label
                      >

                      <input
                        type="number"
                        v-model="rightWallRO4Width"
                        min="12"
                        max="72"
                        step="0.25"
                      />
                      <div v-if="rightWindowWidthWarning" class="warning_box">
                        ⚠️ The selected opening width leaves less than 10" of
                        available wall space, which may be insufficient room for
                        a {{ rightWallRO4Width }}" opening. The Fusion model can
                        safely resolve this condition, however the final design
                        may require adjustments during human review. 🦫
                      </div>

                      <span class="window-legend-width dimension-value">
                        {{ rightWallRO4Width }}"
                      </span>
                    </div>

                    <!-- RO HEIGHT -->

                    <div class="dimension-row">
                      <label class="window-legend-height dimension-label"
                        >RO Height:</label
                      >

                      <input
                        type="number"
                        v-model="rightWallRO4Height"
                        min="12"
                        max="72"
                        step="0.25"
                      />
                      <div v-if="rightWindowFitWarning" class="warning_box">
                        ⚠️ The selected rough opening height, distance from
                        floor, and header size may exceed the available wall
                        height. The Fusion model includes safeguards and may
                        adjust the design, however final placement may require
                        review.
                      </div>

                      <span class="window-legend-height dimension-value">
                        {{ rightWallRO4Height }}"
                      </span>
                    </div>

                    <!-- DISTANCE FROM FLOOR -->

                    <div class="dimension-row">
                      <label class="window-legend-dff dimension-label"
                        >Distance From Floor:</label
                      >

                      <input
                        type="number"
                        v-model="rightWallRO4FloorHeight"
                        min="0"
                        max="72"
                        step="0.25"
                      />

                      <span class="window-legend-dff dimension-value">
                        {{ rightWallRO4FloorHeight }}"
                      </span>
                    </div>

                    <!-- HEADER SIZE -->

                    <div class="dimension-row">
                      <label class="window-legend-headers dimension-label"
                        >Header Size:</label
                      >

                      <select v-model="rightWallRO4HeaderSize">
                        <option value="2x4">2x4</option>
                        <option value="2x6">2x6</option>
                      </select>
                      <span class="window-legend-headers dimension-value">
                        {{ rightWallRO4HeaderSize }}
                      </span>
                    </div>

                    <!-- STUD INDEX -->

                    <div class="dimension-row">
                      <label class="window-legend-stud-index dimension-label"
                        >Distance From Front Corner:</label
                      >

                      <input
                        type="number"
                        v-model="rightWallRO4StudIndex"
                        min="0"
                        :step="currentStudSpacing"
                      />

                      <span class="window-legend-stud-index dimension-value">
                        {{ rightWallRO4StudIndexDisplay }}
                      </span>
                      <div v-if="rightWindowWarning" class="warning_box">
                        ⚠️ Distance From Front Corner leaves less than 12" of
                        clearance from the back corner, which may be
                        insufficient room for a
                        {{ rightWallRO4Width }}" opening. The Fusion model can
                        safely resolve this condition, however the final design
                        may require adjustments during human review. 🦫
                      </div>
                    </div>
                    <p>
                      <button
                        @click="showMoreAboutRightWallWindow = false"
                        class="clear-window-button"
                      >
                        🪟 Close Right Window Options
                      </button>
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!--=========THIS IS THE CLOSING DIV ABOVE THIS LINE, THE DIV THAT CLOSES CLASS window-section?-->
        </div>
      </div>
    </div>
  </div>
</template>
<!--=====================================Start of Styling===================
=================================================================================-->
/*COLOR SYSTEM: GREEN = BUILDING DEFINITIONS, BROWN = STRUCTURE FOUNDATION FLOOR
STUDS, BLUE = OPENINGS WINDOWS DOORS
====================================================*/

<style scoped>
.heroHeader {
  text-align: center;
}

h1,
h2,
h3,
h4 {
  font-family: "Oswald", sans-serif;
  color: #d8b36a;

  text-shadow:
    1px 1px 0px #5e3d16,
    2px 2px 3px rgba(0, 0, 0, 0.3);

  letter-spacing: 3px;
}

.pickYourPoison,/*these are a heading and a link text*/
.change-building-link {
  font-family: "Oswald", sans-serif;
  color: #d8b36a;
  text-align: center;
  position: relative;
  z-index: 600;
  background: rgba(255, 255, 255, 0.89);
  padding: 4px 12px;
  border-radius: 25px;
  width: fit-content;
  margin: 100px auto 0 auto;
}

.landingPage1 p,
.landingPage2 p,
.landingPage1 li {
  font-family: "Inter", sans-serif;
  font-size: 1.1rem;
  line-height: 1.8;
  font-weight: 500;
}

.landingPage1 button,
.landingPage1 label,
.landingPage2 button,
.landingPage2 label,
.landingPage3 button,
.landingPage3 label,
.options-column button,
.options-column label {
  border-radius: 8px;
  font-family: "Oswald", sans-serif;
  color: #5e3d16; /* darker wood color */
  line-height: 1.6;
}

.faq-q,
.whatIs-q {
  color: #d8b36a;
  font-family: "Oswald", sans-serif;
}

.acceptance-box {
  max-width: 400px;
  margin: 30px auto;

  padding: 20px;

  border: 2px solid #d8b36a;
  border-radius: 12px;

  background: rgba(255, 255, 255, 0.85);

  box-shadow: 4px 6px rgba(0, 0, 0, 0.15);

  text-align: center;
}

.landingPage2 p {
  font-family: "Inter", sans-serif;
  line-height: 1.6;
}

.tape_measure_image {
  display: block;
  width: 100%;
  max-width: 1200px;
  margin: 10px auto;
}

.sticky-tape {
  position: sticky;
  top: 0;
  z-index: 500;
  background: white;
  padding-bottom: 0;
}

.scopeBuilding-card {
  border: 2px solid #5e3d16; /* darker wood color */
  border-radius: 8px;
  box-shadow: 4px 6px rgba(0, 0, 0, 0.15);
  padding: 15px;
  margin-top: 4px;
  box-sizing: border-box;
  text-align: center;
}

.building-card {
  border: 2px solid #5e3d16; /* darker wood color */
  border-radius: 8px;
  box-shadow: 4px 6px rgba(0, 0, 0, 0.15);
  padding: 16px;
  margin-top: 12px;
  box-sizing: border-box;
}

.branding-header {
  /*border: 2px solid#d8b36a;*/
  position: sticky;
  top: 0;
  z-index: 1100;
  font-family: "Oswald", sans-serif;
  color: #d8b36a;
  text-align: center;
  background: rgba(255, 255, 255, 0.89);
  padding: 4px 8px;
  border-radius: 25px;
  width: fit-content;
  max-width: calc(100vw - 32px);
  margin-left: auto;
  margin-right: auto;
}
.page2-brandingHeader {
  max-width: 700px;
}

.page3-brandingHeader {
  max-width: 425px;
  top: 8px;
}

.targeted-building-container {
  /*border: 10px solid rgb(226, 80, 80);*/
  min-width: 0;
  z-index: 1000;
  background: transparent;
  width: fit-content;
  max-width: 425px;
  margin: 8px auto 0;
  box-sizing: border-box;
}

.targetedBuilding-card {
  /*border: 2px solid #5e3d16; /* darker wood color */
  border-radius: 8px;
  box-shadow: 4px 6px #5e3d16; /* darker wood color */
  padding: 2px;
  align-self: start;
  min-width: 0;
}

.change-building-link {
  pointer-events: auto;
}

.configurator-layout {
  display: grid;
  grid-template-columns: minmax(280px, 38%) minmax(0, 62%);
  gap: 20px;
  align-items: start;
}
/*========================================THIS IS THE START OF PAGE 3=============
=================================================================================*/
/*======GLOBAL CSS FOR PAGE 3==========*/

.landingPage3 {
  position: sticky;
  top: 8px;
  align-self: start;
  max-height: calc(100vh - 16px);
  overflow-y: visible;
  min-width: 0;
}

.page3-sidebar {
  /*border: 8px solid brown; /* darker wood color */
  border-radius: 8px;
  box-shadow: 4px 6px #5e3d16; /* darker wood color */
  min-width: 0;
  padding: 10px;
  margin-top: 8px;
}

.collapse-thanks-button {
  display: block;
  margin: 8px auto 0;
}

.page3-tape-image {
  display: block;
  width: 75%;
  max-width: 420px;
  margin: 0 auto 16px;
}

.reference-image-container {
  /*border: solid red 3px;*/
  border-radius: 30px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
  padding-top: 0px;
}
/*and because they don't fill the container (wierd corner instances)
======these are the Fusion screen shots==========*/
.reference-image-container img {
  /*border: 3px solid blue;*/
  display: block;
  max-width: 100%;
  width: auto;
  border-radius: 0;
}

.warning_box {
  background-color: #fff3cd;
  border: 1px solid #ffc107;
  border-radius: 8px;
  padding: 12px;
  margin: 12px 0;
  color: #856404;
  font-weight: bold;
}
/*================================CURRENT SELECTION IS A GLOBAL DEFINITION================
FOR THE HEADER THAT SHOWS WHAT THE USER SELECTED. YOU WILL SEE IT BETWEEN 2X4 SKID TYPE ETC.======*/
.current_selection {
  /*border: solid 3px #d86aaa;*/
  font-size: 1.2rem;
  text-align: center;
  color: #d8b36a;
  text-shadow: 1px 1px 1px black;
  margin: 8px 0;
}
/*======================================CONTROL PANEL===============================
====================================================================================*/

.page3-control-panel {
  /*border: solid 6px yellow;*/
  max-height: 53vh;
  min-height: 180px;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  font-family: "Inter", sans-serif;
  line-height: 1.6;
  overflow-y: auto;
}

.dashboard-header {
  position: sticky;
  top: 0;
  background: inherit; /* match panel background */
  z-index: 10;
  padding-bottom: 10px;
}

.dashboard-header h2,
h4 {
  margin: 0;
}

.CP-summary-row {
  display: flex;
  justify-content: space-between;
}

/*=============================summary sections are in the control panel===================*/
.dimensions-summary-section {
  border-left: 6px solid rgb(120, 150, 120);
  background-color: rgba(168, 192, 168, 0.12);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
  color: #462a06;
}

.foundation-summary-section {
  /*border-left: 6px solid #c19a6b;*/
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;

  background-image: url("./assets/Wood004_4K-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  /*border: 3px solid #d8b36a;*/
  color: #e7d9c6;
}

.floor-joist-summary-section {
  border-left: 6px solid #c19a6b;
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;

  background-image: url("./assets/Wood034_4k-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  /*border: 3px solid #d8b36a;*/
  /*color: black;*/
  color: #462a06;
}

.plywood-summary-section {
  /*order-left: 6px solid #c19a6b;*/
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;

  background-image: url("./assets/Chipboard004_4K-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  /*border: 3px solid #d8b36a;*/
  /*color: black;*/
  color: #462a06;
}

.stud-summary-section {
  border-left: 6px solid #c19a6b;
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;

  background-image: url("./assets/Wood034_4k-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  /*border: 3px solid #d8b36a;*/
  /*color: black;*/
  color: #462a06;
}

.roof-peak-summary-section {
  border-left: 6px solid #7d8590;
  background-color: rgba(125, 133, 144, 0.12);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.roof-rafters-summary-section {
  border-left: 6px solid #c19a6b;
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;

  background-image: url("./assets/Wood034_4k-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  /*border: 3px solid #d8b36a;*/
  color: #462a06;
}

.roof-pitch-summary-section {
  border-left: 6px solid #7d8590;
  background-color: rgba(125, 133, 144, 0.12);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
  color: #462a06;
}

.front-door-summary-section {
  border-left: 6px solid #c19a6b;
  background-color: #eee2d9;
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
  color: #462a06;

  /*border: solid magenta 3px;*/
}

.left-window-summary-section {
  border-left: 6px solid #4a90e2;
  background-color: rgba(74, 144, 226, 0.07);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
  color: #462a06;
}

.right-window-summary-section {
  border-left: 6px solid #4a90e2;
  background-color: rgba(74, 144, 226, 0.16);
  border-radius: 6px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
  color: #462a06;
}

.load-mechanism-button {
  position: sticky;
  bottom: 0;
  flex: 0 0 auto;
  width: calc(100% - 16px);
  margin: 8px;
  z-index: 2;
  box-sizing: border-box;
}

/*.load-mechanism-button  {
  hover: background-color: #086e5d;
  color: #086e47;
  z-index: 2;
}*/

.next-step-link {
  /*position: sticky;*/
  bottom: 0;
  flex: 0 0 auto;
  width: calc(100% - 16px);
  margin: 8px;
  z-index: 2;
  box-sizing: border-box;
  /*border: solid red 5px;*/
  text-decoration: none;
  color: #462a06;
  position: relative;
}

.next-step-link::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: 0;
  width: 45%;
  height: 2px;
  background: currentColor;

  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s ease-in;
}

.next-step-link:hover::after {
  transform: scaleX(1);
  transform-origin: left;
}
/*====================================THIS IS RHS STYLES===================*/
.options-column {
  min-width: 0;
  padding: 10px;
}

.main-column {
  font-family: "Inter", sans-serif;
  min-width: 0;
  padding: 10px;
  /*border: 3px solid #5e3d16; /* this can be used for de-bugging, darker wood color */
  border-radius: 8px;
  /*box-shadow: 4px 6px #5e3d16; /* this can be used for de-bugging, darker wood color */
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/*======================================TRAVELER Q CARDS ARE THE ORIGINAL
=========================================NAMED CARDS UNTIL THEY STARTED GETTING
=========================================DIVIDED INTO CATEGORIES AND COLORS APPLIED==WIPROGRESS*/
.travelerQ-card {
  border: 1px solid #5e3d16; /* darker wood color */
  border-radius: 8px;
  padding: 16px;
  /*margin-top: 12px;*/
  box-shadow: 4px 6px #5e3d16;
  margin-top: 0;
  margin-bottom: 24px;
}
/*===================================DIMENSIONS SECTION======================*/

.dimensions_reference_image {
  /*there are currently 3 other instances of this. two iso above the dimensions legend,
   left window and right window images */
  width: 100%;
  max-width: 600px;
  display: block;
  margin-right: auto;
}

.dimension-legend {
  display: flex;
  justify-content: center;
  gap: 40px;
  margin-bottom: 40px;
  margin: 0;
  padding-top: 10px;
}

.legend-width {
  border-left: 8px solid red;
  color: red;
  text-shadow: 1px 1px 1px black;
  padding-left: 10px;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 12px;
}

.traveler-dimensions-Q-card {
  /*background-color: rgba(168, 192, 168, 0.12);*/
  border-radius: 6px;
  padding: 8px;
  margin-top: 0;
  margin-bottom: 24px;
}

.dimensions-section {
  /*border: 3px solid red;*/
  box-shadow: 4px 6px rgb(120, 150, 120);
  background-color: rgba(168, 192, 168, 0.12);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.dimension-row {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.dimension-label {
  width: 100px;
}

.dimension-row input {
  width: 50px;
  text-align: center;
}

.dimension-value {
  width: 250px;
  margin-left: 10px;
}

/*=================================================FOUNDATION SECTION=============*/

.foundation-section {
  box-shadow: 4px 6px #c19a6b;
  /*border: 3px solid blue;*/
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.skids_reference_image {
  width: 100%;
  max-width: 400px;
  display: block;
  margin-right: auto;
}
/*===================================================FLOOR JOIST SECTION=================*/
.floor-joist-section {
  box-shadow: 4px 6px #c19a6b;
  /*border-left: 6px solid #c19a6b;*/
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}
/*===================================================PLYWOOD SECTION=================*/
.plywood-section {
  box-shadow: 4px 6px #c19a6b;
  /*border-left: 6px solid #c19a6b;*/
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}
/*===================================================STUD SECTION=================*/
.stud-section {
  box-shadow: 4px 6px #c19a6b;
  /*border-left: 6px solid #c19a6b;*/
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.traveler-2x4stud-Q-card {
  border-radius: 8px;
  padding: 16px;
  border: 2px solid #5e3d16; /* darker wood color */
  box-shadow: 4px 6px #5e3d16; /* darker wood color */
  background-color: rgba(193, 154, 107, 0.12);
  /*background-size: cover;
  background-position: center; these would go if there was a background image added*/
  margin-top: 0;
  margin-bottom: 24px;
}

.traveler-2x6stud-Q-card {
  border-radius: 8px;
  padding: 16px;

  border: 2px solid #5e3d16; /* darker wood color */
  box-shadow: 4px 6px #5e3d16; /* darker wood color */

  /*background-size: cover;
  background-position: center;*/
  background-color: rgba(193, 154, 107, 0.12);
  margin-top: 0;
  margin-bottom: 24px;
}

.stud-spacing-row {
  display: flex;
  align-items: center;
  gap: 12px;
}
/*==================================ROOF SECTION=======================*/
.roof-peak-section {
  box-shadow: 4px 6px #7d8590;
  /*border-left: 6px solid #c19a6b;*/
  background-color: rgba(125, 133, 144, 0.12);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.roof_direction_image {
  width: 120px;
  flex-shrink: 0;
}

.roof_option {
  display: flex;
  align-items: center;
  gap: 15px;
}

.selected_roofPeak {
  border: 3px solid #7d8590;
  border-radius: 8px;
  box-shadow: 10px 12px #7d8590;
  background-color: rgba(125, 133, 144, 0.08);
}

.roof-rafters-section {
  box-shadow: 4px 6px #c19a6b;
  /*border-left: 6px solid #c19a6b;*/
  background-color: rgba(193, 154, 107, 0.12);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.roof-pitch-section {
  box-shadow: 4px 6px #7d8590;
  /*border-left: 6px solid #c19a6b;*/
  background-color: rgba(125, 133, 144, 0.12);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}
/*=======================================FRONT DOOR SECTION===================*/
.door-section-container {
  /*border: solid magenta 3px;*/
  box-shadow: 4px 6px #c19a6b;
  background-color: #eee2d9;
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.door-section {
  border: solid #c19a6b 3px;
  border-radius: 10px;
  padding: 8px;
  margin-top: 12px;
  margin-bottom: 12px;
}

.traveler-frontDoor-Q-card {
  /*this is the inner card the holds the "Add Front Door" header*/
  border: 2px solid #c19a6b;
  border-radius: 8px;
  padding: 16px;

  box-shadow: 4px 6px #c19a6b;
  margin-top: 0;
  margin-bottom: 24px;
}
/*========================================WINDOW SECTION================*/
.window-section-container {
  box-shadow: 4px 6px #4a90e2;
  background-color: rgba(74, 144, 226, 0.07);
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.window-section {
  box-shadow: 4px 6px rgb#4a902a;
  /*border-left: 6px solid #4a90e2;*/
  /*background-color: rgba(74, 144, 226, 0.07);*/
  border-radius: 10px;
  padding: 8px;
  margin-top: 6px;
  margin-bottom: 6px;
}

.left-window-dimension-legend {
  /*display: flex;*/
  /*justify-content: flex-start;*/
  font-size: 1rem;
}

.right-window-dimension-legend {
  /*display: flex;*/
  /*justify-content: flex-end;*/
  font-size: 1rem;
}

.window-legend-width {
  border-left: 8px solid rgb(117, 238, 117);
  color: rgb(117, 238, 117) !important;
  text-shadow: 1px 1px 1px black;
  padding-left: 10px;
  /*font-size: 1.3rem;*/
  font-weight: bold;
  margin-bottom: 12px;
}

.window-legend-height {
  border-left: 8px solid blue;
  color: blue !important;
  text-shadow: 1px 1px 1px black;
  padding-left: 10px;
  /*font-size: 1.3rem;*/
  font-weight: bold;
  margin-bottom: 12px;
}

.window-legend-headers {
  border-left: 8px solid red;
  color: red !important;
  text-shadow: 1px 1px 1px black;
  padding-left: 10px;
  /*font-size: 1.3rem;*/
  font-weight: bold;
  margin-bottom: 12px;
}

.window-legend-stud-index {
  border-left: 8px solid yellow;
  color: yellow !important;
  text-shadow: 1px 1px 1px black;
  padding-left: 10px;
  /*font-size: 1.3rem;*/
  font-weight: bold;
  margin-bottom: 12px;
}

.window-legend-dff {
  border-left: 8px solid magenta;
  color: magenta !important;
  text-shadow: 1px 1px 1px black;
  padding-left: 10px;
  /*font-size: 1.3rem;*/
  font-weight: bold;
  margin-bottom: 12px;
}

.window-widthDimension-label {
  color: rgb(117, 238, 117);
  font-weight: bold;
}

.legend-depth {
  border-left: 8px solid rgb(93, 245, 93);
  color: rgb(93, 245, 93);
  text-shadow: 1px 1px 1px black;
  padding-left: 10px;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 12px;
}

.legend-height {
  border-left: 8px solid blue;
  color: blue;
  text-shadow: 1px 1px 1px black;
  padding-left: 10px;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 12px;
}

.left-traveler-windowQ-card {
  border-radius: 8px;
  padding: 16px;
  /*margin-top: 12px;*/
  border: 2px solid #4a90e2;
  box-shadow: 4px 6px #4a90e2;
  background-color: rgba(74, 144, 226, 0.07);
  /*background-size: cover;
  background-position: center; these would go if there was a background image added*/
  margin-top: 0;
  margin-bottom: 24px;
}

.right-traveler-windowQ-card {
  border-radius: 8px;
  padding: 16px;
  /*margin-top: 12px;*/
  border: 2px solid #4a90e2;
  box-shadow: 4px 6px #4a90e2;
  background-color: rgba(74, 144, 226, 0.07);
  margin-top: 0;
  margin-bottom: 24px;
}

.left_selected_window {
  border: 5px solid #4a90e2;
  box-shadow: 10px 12px #4a90e2;

  background-color: rgba(74, 144, 226, 0.16);

  /*background-image: ;*/
  /*background-size: cover;
  background-position: center;*/
}

.selected_wood004 {
  background-image: url("./assets/Wood004_4K-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  /*border: 3px solid #d8b36a;*/
  color: white;
}

.selected_wood034 {
  background-image: url("./assets/Wood034_4K-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  border: 3px solid #d8b36a;
  color: white;
}

.selected_osb {
  background-image: url("./assets/Chipboard004_4K-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  border: 3px solid #d8b36a;
  color: rgb(43, 4, 4);
}

.right_selected_window {
  border: 5px solid #4a90e2;
  box-shadow: 10px 12px #4a90e2;

  background-color: rgba(74, 144, 226, 0.16);
  /*background-image: ;*/
  background-size: cover;
  background-position: center;
}

@media (max-width: 800px) {
  .configurator-layout {
    grid-template-columns: 1fr;
  }

  .landingPage3 {
    position: static;
    max-height: none;
  }

  .page3-sidebar {
    position: static;
  }

  .page3-tape-image {
    max-width: 75%;
  }
}
</style>
