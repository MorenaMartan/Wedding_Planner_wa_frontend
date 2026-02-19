<template>
  <div class="content-panel rounded-4 p-3 d-flex flex-column">
    <div class="d-flex flex-column flex-md-row gap-2 mb-3">
      <div
        v-if="
          showSalons ||
          showRings ||
          showShoes ||
          showHairdresser ||
          showCozmetic ||
          showTie ||
          showShoesG ||
          showShirts ||
          showSuit ||
          showHairdresserG ||
          showBarber ||
          showBouquet ||
          showCarFlowers ||
          showHallFlowers ||
          showChurchFlowers ||
          showMusic ||
          showHalls ||
          showCakes ||
          showBands
        "
        class="flex-grow-1 position-relative"
      >
        <input
          v-model="search"
          type="text"
          class="form-control rounded-pill ps-5"
          placeholder="Search..."
        />
        <span class="search-icon">🔍</span>
      </div>

      <select
        v-if="
          showSalons ||
          showRings ||
          showShoes ||
          showHairdresser ||
          showCozmetic ||
          showTie ||
          showShoesG ||
          showShirts ||
          showSuit ||
          showHairdresserG ||
          showBarber ||
          showBouquet ||
          showCarFlowers ||
          showHallFlowers ||
          showChurchFlowers ||
          showMusic ||
          showHalls ||
          showCakes ||
          showBands
        "
        class="form-select rounded-pill w-auto"
        v-model="sortBy"
      >
        <option value="">Sort by</option>
        <option value="name">Name</option>
        <option value="rating">Rating</option>
      </select>
    </div>

    <div v-if="selectedItem" class="flex-grow-1 overflow-auto pe-1">
      <SalonDetails
        v-if="selectedItem.type === 'salon'"
        :salon="selectedItem.data"
        @back="closeDetails"
        @open-chat="openChatForItem"
      />
      <GenericDetails
        v-else
        :item="selectedItem.data"
        :field-label="getFieldLabel(selectedItem.type)"
        @back="closeDetails"
        @open-chat="openChatForItem"
      />
    </div>
    <template v-else>
      <div v-if="showSalons" class="flex-grow-1 overflow-auto pe-1">
        <SalonCard
          v-for="salon in filteredSalons"
          :key="salon.id"
          :salon="salon"
          @read-more="openItemDetails('salon', salon)"
        />
      </div>

      <div v-else-if="showRings" class="flex-grow-1 overflow-auto pe-1">
        <RingsCard
          v-for="store in filteredStores"
          :key="store.id"
          :store="store"
          @read-more="openItemDetails('rings', store)"
        />
      </div>

      <div v-else-if="showShoes" class="flex-grow-1 overflow-auto pe-1">
        <ShoesCard
          v-for="shop in filteredShops"
          :key="shop.id"
          :store="shop"
          @read-more="openItemDetails('shoes', shop)"
        />
      </div>

      <div v-else-if="showHairdresser" class="flex-grow-1 overflow-auto pe-1">
        <HairdresserCard
          v-for="salon in filteredHairSalon"
          :key="salon.id"
          :store="salon"
          @read-more="openItemDetails('hairdresser', salon)"
        />
      </div>

      <div v-else-if="showCozmetic" class="flex-grow-1 overflow-auto pe-1">
        <CozmeticCard
          v-for="salon in filteredCozmetic"
          :key="salon.id"
          :store="salon"
          @read-more="openItemDetails('cozmetic', salon)"
        />
      </div>

      <div v-else-if="showMaidOfHonor" class="flex-grow-1 overflow-auto pe-1">
        <MaidOfHonorCard />
      </div>

      <div v-else-if="showTie" class="flex-grow-1 overflow-auto pe-1">
        <TieCard
          v-for="item in filteredTie"
          :key="item.id"
          :tie="item"
          @read-more="openItemDetails('tie', item)"
        />
      </div>

      <div v-else-if="showShoesG" class="flex-grow-1 overflow-auto pe-1">
        <ShoesGCard
          v-for="item in filteredStoresG"
          :key="item.id"
          :storeg="item"
          @read-more="openItemDetails('shoesG', item)"
        />
      </div>

      <div v-else-if="showShirts" class="flex-grow-1 overflow-auto pe-1">
        <ShirtCard
          v-for="shirt in filteredShirts"
          :key="shirt.id"
          :shirt="shirt"
          @read-more="openItemDetails('shirt', shirt)"
        />
      </div>

      <div v-else-if="showSuit" class="flex-grow-1 overflow-auto pe-1">
        <SuitCard
          v-for="item in filteredSuits"
          :key="item.id"
          :suit="item"
          @read-more="openItemDetails('suit', item)"
        />
      </div>

      <div v-else-if="showHairdresserG" class="flex-grow-1 overflow-auto pe-1">
        <HairdresserGCard
          v-for="hairdresser in filteredHairdresserG"
          :key="hairdresser.id"
          :hairdresser="hairdresser"
          @read-more="openItemDetails('hairdresserG', hairdresser)"
        />
      </div>

      <div v-else-if="showBarber" class="flex-grow-1 overflow-auto pe-1">
        <BarberCard
          v-for="barber in filteredBarbers"
          :key="barber.id"
          :barber="barber"
          @read-more="openItemDetails('barber', barber)"
        />
      </div>
      <div v-else-if="showBestMan" class="flex-grow-1 overflow-auto pe-1">
        <BestManCard />
      </div>

      <div v-else-if="showLapel" class="flex-grow-1 overflow-auto pe-1">
        <LapelCard />
      </div>
      <div v-else-if="showBouquet" class="flex-grow-1 overflow-auto pe-1">
        <BouquetCard
          v-for="flower in filteredFlowers"
          :key="flower.id"
          :flower="flower"
          @read-more="openItemDetails('bouquet', flower)"
        />
      </div>

      <div v-else-if="showCarFlowers" class="flex-grow-1 overflow-auto pe-1">
        <CarFlowersCard
          v-for="car in filteredCarFlowers"
          :key="car.id"
          :car="car"
          @read-more="openItemDetails('carFlowers', car)"
        />
      </div>
      <div v-else-if="showHallFlowers" class="flex-grow-1 overflow-auto pe-1">
        <HallFlowersCard
          v-for="hall in filteredHallFlowers"
          :key="hall.id"
          :hall="hall"
          @read-more="openItemDetails('hallFlowers', hall)"
        />
      </div>
      <div v-else-if="showChurchFlowers" class="flex-grow-1 overflow-auto pe-1">
        <ChurchFlowersCard
          v-for="church in filteredChurchFlowers"
          :key="church.id"
          :church="church"
          @read-more="openItemDetails('churchFlowers', church)"
        />
      </div>

      <div v-else-if="showDateAndTime" class="flex-grow-1 overflow-auto pe-1">
        <DateAndTimeCard />
      </div>

      <div v-else-if="showEngagement" class="flex-grow-1 overflow-auto pe-1">
        <EngagementCard />
      </div>

      <div v-else-if="showMusic" class="flex-grow-1 overflow-auto pe-1">
        <MusicCard
          v-for="music in filteredMusic"
          :key="music.id"
          :music="music"
          @read-more="openItemDetails('music', music)"
        />
      </div>

      <div
        v-else-if="category === 'church' && sub === 'A Priest'"
        class="flex-grow-1 overflow-auto pe-1"
      >
        <APriestCard />
      </div>

      <div
        v-else-if="category === 'church' && sub === 'Readers'"
        class="flex-grow-1 overflow-auto pe-1"
      >
        <ReadersCard />
      </div>

      <div v-else-if="showDocuments" class="flex-grow-1 overflow-auto pe-1">
        <DocumentCard />
      </div>

      <div v-else-if="showConfetti" class="flex-grow-1 overflow-auto pe-1">
        <ConfettiCard />
      </div>

      <div v-else-if="showHalls" class="flex-grow-1 overflow-auto pe-1">
        <HallCard
          v-for="hall in filteredHalls"
          :key="hall.id"
          :hall="hall"
          @read-more="openItemDetails('hall', hall)"
        />
      </div>

      <div v-else-if="showMenu" class="flex-grow-1 overflow-auto pe-1">
        <MenuCard />
      </div>

      <div v-else-if="showCakes" class="flex-grow-1 overflow-auto pe-1">
        <CakeCard
          v-for="cake in filteredCakes"
          :key="cake.id"
          :cake="cake"
          @read-more="openItemDetails('cake', cake)"
        />
      </div>

      <div v-else-if="showBands" class="flex-grow-1 overflow-auto pe-1">
        <BandCard
          v-for="band in filteredBands"
          :key="band.id"
          :band="band"
          @read-more="openItemDetails('band', band)"
        />
      </div>

      <div v-else-if="showDance" class="flex-grow-1 overflow-auto pe-1">
        <DanceCard />
      </div>

      <div v-else-if="showBudget" class="flex-grow-1 overflow-auto pe-1">
        <div class="bg-white rounded-4 shadow-sm p-3">
          <h5 class="fw-bold mb-3 text-center">Wedding Budget Items</h5>

          <div class="budget-table-wrapper">
            <table class="table table-sm align-middle text-center mb-0">
              <thead class="table-light sticky-top">
                <tr>
                  <th class="text-start">Item</th>
                  <th style="width: 120px">Planned (€)</th>
                  <th style="width: 120px">Spent (€)</th>
                  <th style="width: 180px">Document</th>
                  <th style="width: 110px">Remaining</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in budgetItems" :key="item.label">
                  <td class="text-start">
                    <span class="d-flex align-items-center gap-2">
                      <img
                        v-if="item.icon"
                        :src="item.icon"
                        alt=""
                        class="icon-img"
                      />
                      {{ item.label }}
                    </span>
                  </td>
                  <td>
                    <input
                      type="number"
                      class="form-control form-control-sm text-center"
                      v-model.number="item.planned"
                    />
                  </td>
                  <td>
                    <input
                      type="number"
                      class="form-control form-control-sm text-center"
                      v-model.number="item.spent"
                    />
                  </td>
                  <td>
                    <input type="file" class="form-control form-control-sm" />
                  </td>
                  <td>
                    <span
                      :class="{
                        'text-success': item.planned - item.spent > 0,
                        'text-danger': item.planned - item.spent < 0,
                      }"
                    >
                      {{ item.planned - item.spent }}
                    </span>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div v-else-if="showNotes" class="flex-grow-1 p-3 d-flex flex-column">
        <div
          class="bg-white rounded-4 shadow-sm p-3 h-100 d-flex flex-column gap-3"
        >
          <div class="d-flex gap-2">
            <input
              v-model="currentNote.title"
              type="text"
              class="form-control"
              placeholder="Note title..."
            />
            <select
              v-model="currentNote.priority"
              class="form-select"
              style="max-width: 150px"
            >
              <option value="high">High</option>
              <option value="medium">Medium</option>
              <option value="low">Low</option>
            </select>
            <div class="form-check d-flex align-items-center gap-2">
              <input
                type="checkbox"
                class="form-check-input"
                v-model="currentNote.done"
                :id="'note-done-' + currentNote.id"
              />
              <label
                class="form-check-label small"
                :for="'note-done-' + currentNote.id"
              >
                Done
              </label>
            </div>
          </div>

          <textarea
            v-model="currentNote.text"
            class="form-control flex-grow-1"
            placeholder="Write your note here..."
          ></textarea>

          <div class="d-flex gap-3 align-items-center">
            <div>
              <label class="form-label small mb-1">Attach image</label>
              <input type="file" class="form-control form-control-sm" />
            </div>
            <div
              v-if="currentNote.image"
              class="border rounded p-1"
              style="width: 90px; height: 90px; overflow: hidden"
            >
              <img
                :src="currentNote.image"
                alt=""
                class="w-100 h-100 object-fit-cover"
              />
            </div>
          </div>
        </div>
      </div>

      <div
        v-else-if="showGuests"
        class="flex-grow-1 d-flex flex-column overflow-auto pe-1"
      >
        <div class="d-flex flex-wrap gap-2 mb-3">
          <select
            v-model="guestFilterStatus"
            class="form-select form-select-sm w-auto rounded-pill"
          >
            <option value="all">All statuses</option>
            <option value="invited">Invited</option>
            <option value="confirmed">Confirmed</option>
            <option value="declined">Declined</option>
            <option value="maybe">Maybe</option>
          </select>

          <select
            v-model="guestFilterSide"
            class="form-select form-select-sm w-auto rounded-pill"
          >
            <option value="all">Both sides</option>
            <option value="bride">Bride's side</option>
            <option value="groom">Groom's side</option>
          </select>

          <select
            v-model="guestSort"
            class="form-select form-select-sm w-auto rounded-pill ms-auto"
          >
            <option value="name-asc">Name A–Z</option>
            <option value="name-desc">Name Z–A</option>
            <option value="status">By status</option>
            <option value="side">By side</option>
          </select>
        </div>

        <div class="bg-white rounded-4 shadow-sm p-3 flex-grow-1 overflow-auto">
          <table class="table table-sm align-middle">
            <thead class="table-light">
              <tr>
                <th>Name</th>
                <th>Side</th>
                <th>Group</th>
                <th>Status</th>
                <th class="text-center">Seats</th>
                <th>Note</th>
                <th class="text-center" style="width: 70px">Actions</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="guest in filteredGuests" :key="guest.id">
                <td>{{ guest.name }}</td>
                <td class="text-capitalize">{{ guest.side }}</td>
                <td class="text-capitalize">{{ guest.group }}</td>
                <td>
                  <select
                    v-model="guest.status"
                    class="form-select form-select-sm"
                  >
                    <option value="invited">Invited</option>
                    <option value="confirmed">Confirmed</option>
                    <option value="declined">Declined</option>
                    <option value="maybe">Maybe</option>
                  </select>
                </td>
                <td class="text-center">
                  <input
                    type="number"
                    min="1"
                    class="form-control form-control-sm text-center"
                    v-model.number="guest.seats"
                  />
                </td>
                <td>
                  <input
                    type="text"
                    class="form-control form-control-sm"
                    v-model="guest.note"
                    placeholder="Note..."
                  />
                </td>
                <td class="text-center">
                  <button
                    class="btn btn-sm p-0 px-1 guest-delete-btn"
                    @click="deleteGuest(guest.id)"
                  >
                    <img :src="trash" alt="delete" class="guest-delete-icon" />
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <div class="mt-3 bg-white rounded-4 shadow-sm p-3">
          <h6 class="fw-bold mb-2">Add guest</h6>
          <div class="row g-2">
            <div class="col-md-3">
              <input
                v-model="newGuest.name"
                type="text"
                class="form-control form-control-sm"
                placeholder="Name and surname"
              />
            </div>
            <div class="col-md-2">
              <select
                v-model="newGuest.side"
                class="form-select form-select-sm"
              >
                <option value="bride">Bride</option>
                <option value="groom">Groom</option>
              </select>
            </div>
            <div class="col-md-2">
              <select
                v-model="newGuest.group"
                class="form-select form-select-sm"
              >
                <option value="family">Family</option>
                <option value="friends">Friends</option>
                <option value="colleagues">Colleagues</option>
                <option value="other">Other</option>
              </select>
            </div>
            <div class="col-md-2">
              <input
                v-model.number="newGuest.seats"
                type="number"
                min="1"
                class="form-control form-control-sm"
                placeholder="Seats"
              />
            </div>
            <div class="col-md-3 d-flex gap-2">
              <input
                v-model="newGuest.note"
                type="text"
                class="form-control form-control-sm"
                placeholder="Note (optional)"
              />
              <button
                class="btn btn-sm btn-primary rounded-pill"
                @click="addGuest"
              >
                Save
              </button>
            </div>
          </div>
        </div>
      </div>
      <div v-else-if="showChat" class="flex-grow-1 d-flex flex-column">
        <div
          class="d-flex align-items-center justify-content-between mb-2 px-2"
        >
          <div>
            <h6 class="mb-0 fw-bold">{{ currentChatTitle }}</h6>
          </div>

          <div class="d-flex align-items-center gap-2">
            <span class="badge bg-success rounded-pill">Online</span>
          </div>
        </div>

        <div
          class="chat-messages flex-grow-1 mb-3 p-3 rounded-4 bg-white shadow-sm overflow-auto"
        >
          <div
            v-for="msg in currentChatMessages"
            :key="msg.id"
            class="d-flex mb-2"
            :class="{
              'justify-content-end': msg.from === 'me',
              'justify-content-start': msg.from === 'them',
            }"
          >
            <div
              class="chat-bubble"
              :class="{
                'chat-bubble-me': msg.from === 'me',
                'chat-bubble-them': msg.from === 'them',
              }"
            >
              <div v-if="msg.type === 'text'">
                {{ msg.text }}
              </div>
              <div v-else-if="msg.type === 'file'">📎 {{ msg.text }}</div>
              <div class="chat-time">{{ msg.time }}</div>
            </div>
          </div>
        </div>

        <div class="chat-input mt-auto">
          <div
            class="bg-white rounded-4 shadow-sm p-2 d-flex align-items-center gap-2"
          >
            <label class="btn btn-light btn-sm mb-0 rounded-circle">
              📎
              <input
                type="file"
                class="d-none"
                multiple
                @change="onChatFilesChange"
              />
            </label>

            <textarea
              v-model="newChatMessage"
              class="form-control border-0 flex-grow-1"
              rows="1"
              placeholder="Write a message..."
              @keyup.enter.exact.prevent="sendChatMessage"
            ></textarea>

            <button
              class="btn btn-primary rounded-pill btn-sm px-3"
              type="button"
              @click="sendChatMessage"
            >
              Send
            </button>
          </div>
        </div>
      </div>
      <div v-else-if="showTheDay" class="flex-grow-1 overflow-auto pe-1">
        <TheDayCard />
      </div>
      <div
        v-else
        class="d-flex flex-column justify-content-center align-items-center h-100 text-center"
      >
        <h4 class="mb-2 text-muted">Please select some tab in left menu</h4>
      </div>
    </template>
  </div>
</template>

<script>
import SalonCard from "./SalonCard.vue";
import RingsCard from "./RingsCard.vue";
import ShoesCard from "./ShoesCard.vue";
import HairdresserCard from "./HairdresserCard.vue";
import CozmeticCard from "./CozmeticCard.vue";
import MaidOfHonorCard from "./MaidOfHonorCard.vue";
import TieCard from "./TieCard.vue";
import ShoesGCard from "./ShoesGCard.vue";
import ShirtCard from "./ShirtCard.vue";
import SuitCard from "./SuitCard.vue";
import HairdresserGCard from "./HairdresserGCard.vue";
import BarberCard from "./BarberCard.vue";
import BestManCard from "./BestManCard.vue";
import LapelCard from "./LapelCard.vue";
import BouquetCard from "./BouquetCard.vue";
import CarFlowersCard from "./CarFlowersCard.vue";
import HallFlowersCard from "./HallFlowersCard.vue";
import ChurchFlowersCard from "./ChurchFlowersCard.vue";
import DateAndTimeCard from "./DateAndTimeCard.vue";
import EngagementCard from "./EngagementCard.vue";
import MusicCard from "./MusicCard.vue";
import APriestCard from "./APriestCard.vue";
import ReadersCard from "./ReadersCard.vue";
import DocumentCard from "./DocumentCard.vue";
import ConfettiCard from "./ConfettiCard.vue";
import HallCard from "./HallCard.vue";
import MenuCard from "./MenuCard.vue";
import CakeCard from "./CakeCard.vue";
import BandCard from "./BandCard.vue";
import DanceCard from "./DanceCard.vue";
import TheDayCard from "./TheDayCard.vue";

import bride from "@/assets/bride.png";
import ring from "@/assets/ring.png";
import hairdresser from "@/assets/hairdresser.png";
import heels from "@/assets/heels.png";
import nails from "@/assets/nails.png";
import maid from "@/assets/maid.png";
import tai from "@/assets/tai.png";
import groom from "@/assets/groom.png";
import shirt from "@/assets/shirt.png";
import suit from "@/assets/suit.png";
import shoes from "@/assets/shoes.png";
import car from "@/assets/car.png";
import barber from "@/assets/barber.png";
import time from "@/assets/time.png";
import church from "@/assets/church.png";
import flowers from "@/assets/flowers.png";
import lapel from "@/assets/lapel.png";
import bouquet from "@/assets/bouquet.png";
import engagement from "@/assets/engagement.png";
import music from "@/assets/music.png";
import priest from "@/assets/priest.png";
import note from "@/assets/note.png";
import readers from "@/assets/readers.png";
import confetti from "@/assets/confetti.png";
import band from "@/assets/band.png";
import menu from "@/assets/menu.png";
import halla from "@/assets/halla.png";
import cake from "@/assets/cake.png";
import dance from "@/assets/dance.png";
import trash from "@/assets/trash.png";

import SalonDetails from "./SalonDetails.vue";
import GenericDetails from "./GenericDetails.vue";
import api from "../api.js";

export default {
  name: "ContentPanel",
  components: {
    SalonDetails,
    GenericDetails,
    SalonCard,
    RingsCard,
    ShoesCard,
    HairdresserCard,
    CozmeticCard,
    MaidOfHonorCard,
    TieCard,
    ShoesGCard,
    ShirtCard,
    SuitCard,
    HairdresserGCard,
    BarberCard,
    BestManCard,
    LapelCard,
    BouquetCard,
    CarFlowersCard,
    HallFlowersCard,
    ChurchFlowersCard,
    DateAndTimeCard,
    EngagementCard,
    MusicCard,
    APriestCard,
    ReadersCard,
    DocumentCard,
    ConfettiCard,
    HallCard,
    MenuCard,
    CakeCard,
    BandCard,
    DanceCard,
    TheDayCard,
  },
  props: {
    category: String,
    sub: String,
    chats: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      search: "",
      sortBy: "",
      selectedItem: null,
      notesData: [
        {
          id: 1,
          title: "Checklist for photographer",
          priority: "high",
          done: false,
          text: "Ask about golden hour, backup, price.",
          image: "",
        },
        {
          id: 2,
          title: "Questions for the hall",
          priority: "medium",
          done: false,
          text: "How many people, cake, music until when...",
          image: "",
        },
        {
          id: 3,
          title: "Guests to call",
          priority: "low",
          done: true,
          text: "Marko, Ana, Luka...",
          image: "",
        },
      ],
      guests: [
        {
          id: 1,
          name: "Marko Markić",
          side: "bride",
          group: "family",
          status: "confirmed",
          seats: 2,
          note: "",
        },
        {
          id: 2,
          name: "Ana Aničić",
          side: "groom",
          group: "friends",
          status: "invited",
          seats: 1,
          note: "",
        },
        {
          id: 3,
          name: "Luka Lukić",
          side: "bride",
          group: "colleagues",
          status: "maybe",
          seats: 1,
          note: "",
        },
      ],

      newGuest: {
        name: "",
        side: "bride",
        group: "family",
        status: "invited",
        seats: 1,
        note: "",
      },

      guestSort: "name-asc",
      guestFilterStatus: "all",
      guestFilterSide: "all",

      budgetItems: [
        { label: "Wedding dress", icon: bride, planned: 0, spent: 0 },
        { label: "Rings", icon: ring, planned: 0, spent: 0 },
        { label: "Shoes (bride)", icon: heels, planned: 0, spent: 0 },
        { label: "Hairdresser (bride)", icon: hairdresser, planned: 0, spent: 0 },
        { label: "Nails", icon: nails, planned: 0, spent: 0 },
        { label: "Maid of Honor", icon: maid, planned: 0, spent: 0 },
        { label: "Tie", icon: tai, planned: 0, spent: 0 },
        { label: "Shoes (groom)", icon: shoes, planned: 0, spent: 0 },
        { label: "Shirt", icon: shirt, planned: 0, spent: 0 },
        { label: "Suit", icon: suit, planned: 0, spent: 0 },
        { label: "Hairdresser (groom)", icon: hairdresser, planned: 0, spent: 0 },
        { label: "Barber", icon: barber, planned: 0, spent: 0 },
        { label: "Best Man", icon: groom, planned: 0, spent: 0 },
        { label: "Bouquet", icon: bouquet, planned: 0, spent: 0 },
        { label: "Lapel", icon: lapel, planned: 0, spent: 0 },
        { label: "Car flowers", icon: car, planned: 0, spent: 0 },
        { label: "Hall flowers", icon: flowers, planned: 0, spent: 0 },
        { label: "Church flowers", icon: church, planned: 0, spent: 0 },
        { label: "Date & Time", icon: time, planned: 0, spent: 0 },
        { label: "Engagement course", icon: engagement, planned: 0, spent: 0 },
        { label: "Music", icon: music, planned: 0, spent: 0 },
        { label: "A Priest", icon: priest, planned: 0, spent: 0 },
        { label: "Readers", icon: readers, planned: 0, spent: 0 },
        { label: "Documents", icon: note, planned: 0, spent: 0 },
        { label: "Confetti", icon: confetti, planned: 0, spent: 0 },
        { label: "Hall", icon: halla, planned: 0, spent: 0 },
        { label: "Menu", icon: menu, planned: 0, spent: 0 },
        { label: "Cake", icon: cake, planned: 0, spent: 0 },
        { label: "Band", icon: band, planned: 0, spent: 0 },
        { label: "First dance", icon: dance, planned: 0, spent: 0 },
      ],
      budgetSaveTimeout: null,

      salons: [],
      stores: [],
      shops: [],
      hairSalon: [],
      cozmeticSalon: [],
      ties: [],
      shopsg: [],
      shirts: [],
      suits: [],
      hairSalonG: [],
      barbers: [],
      flowers: [],
      carFlowers: [],
      hallFlowers: [],
      churchFlowers: [],
      music: [],
      halls: [],
      cakes: [],
      band: [],
      trash,
      chatMessages: {},

      newChatMessage: "",
      newChatFiles: [],
    };
  },
  computed: {
    showChat() {
      return this.category === "chat";
    },
    currentChatId() {
      if (!this.showChat) return null;
      return this.sub && this.sub.startsWith("chat-")
        ? Number(this.sub.replace("chat-", ""))
        : null;
    },
    currentChatTitle() {
      if (!this.showChat) return "Chat";

      const id = this.currentChatId;
      const chat = this.chats.find((c) => c.id === id);
      return chat ? chat.title : "Chat";
    },

    currentChat() {
      if (!this.currentChatId) return null;
      return this.chats.find((c) => c.id === this.currentChatId) || null;
    },
    currentChatMessages() {
      if (!this.currentChat) return [];
      return this.chatMessages[this.currentChat.subjectId] || [];
    },
    showGuests() {
      return this.category === "guests";
    },

    filteredFlowers() {
      let list = this.flowers;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },

    filteredGuests() {
      let list = this.guests;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((g) => g.name.toLowerCase().includes(q));
      }

      if (this.guestFilterStatus !== "all") {
        list = list.filter((g) => g.status === this.guestFilterStatus);
      }

      if (this.guestFilterSide !== "all") {
        list = list.filter((g) => g.side === this.guestFilterSide);
      }

      const sorted = [...list];
      switch (this.guestSort) {
        case "name-desc":
          sorted.sort((a, b) => b.name.localeCompare(a.name));
          break;
        case "status":
          sorted.sort(
            (a, b) =>
              a.status.localeCompare(b.status) || a.name.localeCompare(b.name)
          );
          break;
        case "side":
          sorted.sort(
            (a, b) =>
              a.side.localeCompare(b.side) || a.name.localeCompare(b.name)
          );
          break;
        default:
          sorted.sort((a, b) => a.name.localeCompare(b.name));
      }

      return sorted;
    },
    showSalons() {
      return (
        this.category === "bride" &&
        !["Rings", "Shoes", "Hairdresser", "Nails", "Maid of Honor"].includes(
          this.sub
        )
      );
    },
    showRings() {
      return this.category === "bride" && this.sub === "Rings";
    },
    showShoes() {
      return this.category === "bride" && this.sub === "Shoes";
    },
    showHairdresser() {
      return this.category === "bride" && this.sub === "Hairdresser";
    },
    showCozmetic() {
      return this.category === "bride" && this.sub === "Nails";
    },
    showMaidOfHonor() {
      return this.category === "bride" && this.sub === "Maid of Honor";
    },
    showTie() {
      return this.category === "groom" && this.sub === "Tie";
    },
    showShoesG() {
      return this.category === "groom" && this.sub === "Shoes";
    },
    showShirts() {
      return this.category === "groom" && this.sub === "Shirt";
    },
    showSuit() {
      return this.category === "groom" && this.sub === "Suit";
    },
    showHairdresserG() {
      return this.category === "groom" && this.sub === "Hairdresser";
    },
    showBarber() {
      return this.category === "groom" && this.sub === "Barber";
    },
    showBestMan() {
      return this.category === "groom" && this.sub === "Best Man";
    },
    showLapel() {
      return this.category === "flowers" && this.sub === "Lapel";
    },
    showBouquet() {
      return this.category === "flowers" && this.sub === "Bouquet";
    },
    showCarFlowers() {
      return this.category === "flowers" && this.sub === "Car flowers";
    },
    showHallFlowers() {
      return this.category === "flowers" && this.sub === "Hall flowers";
    },
    showChurchFlowers() {
      return this.category === "flowers" && this.sub === "Church flowers";
    },
    showDateAndTime() {
      return this.category === "church" && this.sub === "Date & Time";
    },
    showEngagement() {
      return this.category === "church" && this.sub === "Engagement course";
    },
    showMusic() {
      return this.category === "church" && this.sub === "Music";
    },
    showReaders() {
      return this.category === "church" && this.sub === "Readers";
    },
    showDocuments() {
      return this.category === "church" && this.sub === "Documents";
    },
    showConfetti() {
      return this.category === "church" && this.sub === "Confetti";
    },

    showHalls() {
      return this.category === "hall" && this.sub === "Hall";
    },

    showMenu() {
      return this.category === "hall" && this.sub === "Menu";
    },

    showCakes() {
      return this.category === "hall" && this.sub === "Cake";
    },

    showBands() {
      return this.category === "hall" && this.sub === "Band";
    },

    showDance() {
      return this.category === "hall" && this.sub === "First dance";
    },

    filteredSalons() {
      let list = this.salons;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },

    filteredStores() {
      let list = this.stores;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },

    filteredShops() {
      let list = this.shops;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredHairSalon() {
      let list = this.hairSalon;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredCozmetic() {
      let list = this.cozmeticSalon;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredTie() {
      let list = this.ties;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredStoresG() {
      let list = this.shopsg;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredShirts() {
      let list = this.shirts;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredSuits() {
      let list = this.suits;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredHairdresserG() {
      let list = this.hairSalonG;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredBarbers() {
      let list = this.barbers;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredCarFlowers() {
      let list = this.carFlowers;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredHallFlowers() {
      let list = this.hallFlowers;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredChurchFlowers() {
      let list = this.churchFlowers;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredMusic() {
      let list = this.music;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredHalls() {
      let list = this.halls;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredCakes() {
      let list = this.cakes;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },
    filteredBands() {
      let list = this.band;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((s) => s.name.toLowerCase().includes(q));
      }

      return this.sortList(list);
    },

    showBudget() {
      return this.category === "budget";
    },
    showTheDay() {
      return this.category === "day";
    },
    showNotes() {
      return this.category === "notes";
    },
    currentNote() {
      if (!this.showNotes) return {};

      const id =
        this.sub && this.sub.startsWith("note-")
          ? Number(this.sub.replace("note-", ""))
          : this.notesData[0]?.id;
      return this.notesData.find((n) => n.id === id) || this.notesData[0];
    },
  },
  async mounted() {
    const categoryMap = {
      salon: "salons",
      rings: "stores",
      shoes: "shops",
      hairdresser: "hairSalon",
      cozmetic: "cozmeticSalon",
      tie: "ties",
      shoesG: "shopsg",
      shirt: "shirts",
      suit: "suits",
      hairdresserG: "hairSalonG",
      barber: "barbers",
      bouquet: "flowers",
      carFlowers: "carFlowers",
      hallFlowers: "hallFlowers",
      churchFlowers: "churchFlowers",
      music: "music",
      hall: "halls",
      cake: "cakes",
      band: "band",
    };

    try {
      const res = await api.get("/vendors");
      const all = res.data.map((v) => ({ ...v, id: v._id }));

      for (const [cat, prop] of Object.entries(categoryMap)) {
        this[prop] = all.filter((v) =>
          Array.isArray(v.category)
            ? v.category.includes(cat)
            : v.category === cat,
        );
      }
    } catch (err) {
      console.error("Failed to load vendors:", err);
    }

    try {
      const budgetRes = await api.get("/budget");
      const saved = budgetRes.data;
      if (saved.length) {
        for (const item of this.budgetItems) {
          const match = saved.find((s) => s.label === item.label);
          if (match) {
            item.planned = match.planned || 0;
            item.spent = match.spent || 0;
          }
        }
      }
      this.emitBudgetTotals();
    } catch (err) {
      console.error("Failed to load budget:", err);
    }
  },
  watch: {
    category() {
      this.selectedItem = null;
    },
    sub() {
      this.selectedItem = null;
    },
    budgetItems: {
      deep: true,
      handler() {
        this.emitBudgetTotals();
        clearTimeout(this.budgetSaveTimeout);
        this.budgetSaveTimeout = setTimeout(() => this.saveBudget(), 800);
      },
    },
    currentChat: {
      immediate: true,
      handler(chat) {
        if (chat && chat.subjectId) {
          this.loadChatMessages(chat.subjectId);
        }
      },
    },
  },
  methods: {
    emitBudgetTotals() {
      const planned = this.budgetItems.reduce((s, i) => s + (i.planned || 0), 0);
      const spent = this.budgetItems.reduce((s, i) => s + (i.spent || 0), 0);
      this.$emit("budget-updated", { planned, spent });
    },
    async saveBudget() {
      try {
        const items = this.budgetItems.map((i) => ({
          label: i.label,
          planned: i.planned || 0,
          spent: i.spent || 0,
        }));
        await api.put("/budget", { items });
      } catch (err) {
        console.error("Failed to save budget:", err);
      }
    },
    sortList(list) {
      if (this.sortBy === "name") {
        return [...list].sort((a, b) => a.name.localeCompare(b.name));
      }
      if (this.sortBy === "rating") {
        return [...list].sort((a, b) => (b.rating || 0) - (a.rating || 0));
      }
      return list;
    },
    openItemDetails(type, item) {
      this.selectedItem = { type, data: item };
    },
    closeDetails() {
      this.selectedItem = null;
    },
    openChatForItem() {
      if (!this.selectedItem) return;

      const { type, data } = this.selectedItem;

      this.$emit("open-chat", {
        type,
        subjectId: data.id,
        title: data.name,
      });
    },
    async loadChatMessages(vendorId) {
      if (!vendorId || this.chatMessages[vendorId]) return;
      try {
        const res = await api.get("/chat?vendor=" + vendorId);
        this.chatMessages[vendorId] = res.data.map((m) => ({
          id: m._id,
          from: m.from,
          text: m.text,
          time: new Date(m.createdAt).toLocaleTimeString("hr-HR", { hour: "2-digit", minute: "2-digit" }),
          type: m.type,
        }));
      } catch (err) {
        console.error("Failed to load chat:", err);
      }
    },
    async sendChatMessage() {
      const text = this.newChatMessage.trim();
      if (!text && !this.newChatFiles.length) return;

      const chat = this.currentChat;
      if (!chat) return;

      const vendorId = chat.subjectId;

      if (!this.chatMessages[vendorId]) {
        this.chatMessages[vendorId] = [];
      }

      if (text) {
        try {
          const res = await api.post("/chat", {
            vendor: vendorId,
            from: "me",
            text,
            type: "text",
          });
          this.chatMessages[vendorId].push({
            id: res.data._id,
            from: "me",
            text,
            time: new Date(res.data.createdAt).toLocaleTimeString("hr-HR", { hour: "2-digit", minute: "2-digit" }),
            type: "text",
          });
        } catch (err) {
          console.error("Failed to send message:", err);
        }
      }

      for (const file of this.newChatFiles) {
        try {
          const res = await api.post("/chat", {
            vendor: vendorId,
            from: "me",
            text: file.name,
            type: "file",
          });
          this.chatMessages[vendorId].push({
            id: res.data._id,
            from: "me",
            text: file.name,
            time: new Date(res.data.createdAt).toLocaleTimeString("hr-HR", { hour: "2-digit", minute: "2-digit" }),
            type: "file",
          });
        } catch (err) {
          console.error("Failed to send file:", err);
        }
      }

      this.newChatMessage = "";
      this.newChatFiles = [];
    },

    onChatFilesChange(event) {
      const files = Array.from(event.target.files || []);
      this.newChatFiles = files;
    },
    deleteGuest(id) {
      this.guests = this.guests.filter((g) => g.id !== id);
    },
    addGuest() {
      if (!this.newGuest.name.trim()) {
        return;
      }

      const id = this.guests.length
        ? Math.max(...this.guests.map((g) => g.id)) + 1
        : 1;

      this.guests.push({
        id,
        ...this.newGuest,
      });

      this.newGuest = {
        name: "",
        side: "bride",
        group: "family",
        status: "invited",
        seats: 1,
        note: "",
      };
    },
    onSelectNote(id) {
      this.activeNoteId = id;
    },
    onAddNote() {
      const newId = this.notes.length
        ? Math.max(...this.notes.map((n) => n.id)) + 1
        : 1;
      this.notes.push({
        id: newId,
        title: "New note",
        priority: "low",
        done: false,
        text: "",
        image: "",
      });
      this.activeNoteId = newId;
    },
    getFieldLabel(type) {
      const labels = {
        rings: "Ring style",
        shoes: "Shoe style",
        hairdresser: "Hairstyle",
        cozmetic: "Treatment",
        tie: "Tie style",
        shoesG: "Shoe style",
        shirt: "Shirt style",
        suit: "Suit style",
        hairdresserG: "Hairstyle",
        barber: "Haircut style",
        bouquet: "Bouquet style",
        carFlowers: "Car decoration",
        hallFlowers: "Hall decoration",
        churchFlowers: "Church decoration",
        music: "Music style",
        hall: "Hall capacity",
        cake: "Cake style",
        band: "Music genre",
      };
      return labels[type] || "Details";
    },
    saveTheDay(locations) {
      localStorage.setItem("theDayTimeline", JSON.stringify(locations));
    },

    loadTheDay() {
      const saved = localStorage.getItem("theDayTimeline");
      return saved ? JSON.parse(saved) : [{ name: "", time: "", address: "" }];
    },
  },
};
</script>

<style scoped>
.content-panel {
  background: rgba(244, 231, 204, 0.95);
  border-radius: 1.5rem;
  height: 100%;
  min-height: calc(100vh - 200px);
  max-height: calc(100vh - 200px);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.content-panel > .flex-grow-1 {
  min-height: 0;
  overflow-y: auto;
}

.search-icon {
  position: absolute;
  top: 50%;
  left: 15px;
  transform: translateY(-50%);
}
.icon-img {
  width: 30px;
  height: 30px;
  object-fit: contain;
}

.budget-table-wrapper {
  overflow: visible;
}

textarea {
  min-height: 160px;
  resize: vertical;
}
.guest-delete-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
}

.guest-delete-btn {
  background: transparent;
  border: none;
}
.chat-messages {
  max-height: calc(100vh - 260px);
}

.chat-bubble {
  max-width: 75%;
  padding: 0.5rem 0.75rem;
  border-radius: 1rem;
  font-size: 0.9rem;
  position: relative;
}
.chat-bubble-me {
  background: #0d1750;
  color: #fff;
  border-bottom-right-radius: 0.2rem;
}
.chat-bubble-them {
  background: #f1f1f1;
  color: #000;
  border-bottom-left-radius: 0.2rem;
}
.chat-time {
  font-size: 0.7rem;
  opacity: 0.7;
  margin-top: 0.15rem;
  text-align: right;
}
.chat-input textarea {
  resize: none;
}
</style>
