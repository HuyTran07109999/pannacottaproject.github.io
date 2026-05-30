const PRODUCTS = [
  { id: "vanilla-1", name: "Panna Cotta Vanilla", price: 39000, desc: "Béo ngậy vừa phải, thơm vanilla.", tag: "classic", badge: "Cổ điển" },
  { id: "strawberry-1", name: "Panna Cotta Dâu", price: 45000, desc: "Sốt dâu đậm vị, hương thơm tự nhiên.", tag: "fruit", badge: "Trái cây" },
  { id: "mango-1", name: "Panna Cotta Xoài", price: 49000, desc: "Vị xoài chín mọng, mịn màng.", tag: "fruit", badge: "Best choice" },
  { id: "choco-1", name: "Panna Cotta Socola", price: 52000, desc: "Socola đậm, béo, ăn là nghiền.", tag: "chocolate", badge: "Socola" },
  { id: "matcha-1", name: "Panna Cotta Trà Xanh", price: 54000, desc: "Hậu vị thanh, thơm nhẹ như trà.", tag: "classic", badge: "Thanh mát" },
  { id: "mix-1", name: "Combo Mix (3 vị)", price: 129000, desc: "Vanilla + Dâu + Xoài (tiết kiệm).", tag: "fruit", badge: "Combo" }
];

let state = {
  filter: "all",
  cart: [] // {id, qty}
};

const els = {
  year: document.getElementById("year"),
  grid: document.getElementById("productGrid"),
  cartCount: document.getElementById("cartCount"),
  cartModal: document.getElementById("cartModal"),
  cartBackdrop: document.getElementById("cartBackdrop"),
  openCartBtn: document.getElementById("openCartBtn"),
  closeCartBtn: document.getElementById("closeCartBtn"),
  cartList: document.getElementById("cartList"),
  cartTotal: document.getElementById("cartTotal"),
  cartEmpty: document.getElementById("cartEmpty"),
  clearCartBtn: document.getElementById("clearCartBtn"),
  checkoutBtn: document.getElementById("checkoutBtn"),
  checkoutHint: document.getElementById("checkoutHint"),
  toast: document.getElementById("toast"),
  productFilterBtns: document.querySelectorAll(".filter-btn"),
  contactForm: document.getElementById("contactForm"),
};

function formatVND(n) {
  return new Intl.NumberFormat("vi-VN").format(n) + "đ";
}

function showToast(msg) {
  els.toast.textContent = msg;
  els.toast.style.display = "block";
  clearTimeout(showToast._t);
  showToast._t = setTimeout(() => {
    els.toast.style.display = "none";
  }, 1800);
}

function getProduct(id) {
  return PRODUCTS.find(p => p.id === id);
}

function getCartQty(id) {
  const item = state.cart.find(x => x.id === id);
  return item ? item.qty : 0;
}

function addToCart(id, qty = 1) {
  const p = getProduct(id);
  if (!p) return;

  const item = state.cart.find(x => x.id === id);
  if (item) item.qty += qty;
  else state.cart.push({ id, qty });

  showToast(`Đã thêm: ${p.name}`);
  renderCart();
}

function updateQty(id, qty) {
  const item = state.cart.find(x => x.id === id);
  if (!item) return;

  item.qty = Math.max(1, qty);
  renderCart();
}

function removeFromCart(id) {
  state.cart = state.cart.filter(x => x.id !== id);
  renderCart();
}

function cartTotal() {
  return state.cart.reduce((sum, it) => {
    const p = getProduct(it.id);
    return sum + (p ? p.price * it.qty : 0);
  }, 0);
}

function cartCountTotal() {
  return state.cart.reduce((sum, it) => sum + it.qty, 0);
}

function renderProducts() {
  const list = PRODUCTS.filter(p => state.filter === "all" ? true : p.tag === state.filter);

  els.grid.innerHTML = list.map(p => {
    return `
      <article class="card">
        <div class="card__top">
          <span class="badge">${p.badge}</span>
        </div>

        <h3 class="productTitle">${p.name}</h3>
        <div class="card__image" aria-hidden="true"></div>
        <p class="productDesc">${p.desc}</p>

        <div class="card__bottom">
          <div class="price">
            <small>Giá</small>
            ${formatVND(p.price)}
          </div>

          <button class="btn btn--primary"
                  type="button"
                  data-add="${p.id}">
            + Thêm
          </button>
        </div>
      </article>
    `;
  }).join("");

  els.grid.querySelectorAll("[data-add]").forEach(btn => {
    btn.addEventListener("click", () => addToCart(btn.getAttribute("data-add"), 1));
  });
}

function renderCart() {
  const total = cartTotal();
  const count = cartCountTotal();

  els.cartCount.textContent = count;
  els.cartTotal.textContent = formatVND(total);

  els.checkoutBtn.disabled = state.cart.length === 0;

  if (state.cart.length === 0) {
    els.cartEmpty.style.display = "block";
    els.cartList.style.display = "none";
  } else {
    els.cartEmpty.style.display = "none";
    els.cartList.style.display = "flex";
  }

  if (state.cart.length === 0) {
    els.cartList.innerHTML = "";
    return;
  }

  els.cartList.innerHTML = state.cart.map(it => {
    const p = getProduct(it.id);
    const line = p ? p.price * it.qty : 0;

    return `
      <div class="cartItem">
        <div class="cartItem__left">
          <b>${p ? p.name : it.id}</b>
          <span>${p ? formatVND(p.price) : ""} • Dòng: ${formatVND(line)}</span>
        </div>

        <div class="cartItem__right">
          <div class="qty">
            <button type="button" data-minus="${it.id}" aria-label="Giảm số lượng">−</button>
            <input type="number" min="1" step="1" value="${it.qty}" data-qty="${it.id}" />
            <button type="button" data-plus="${it.id}" aria-label="Tăng số lượng">+</button>
          </div>
          <button class="remove" type="button" data-remove="${it.id}">Xoá</button>
        </div>
      </div>
    `;
  }).join("");

  els.cartList.querySelectorAll("[data-minus]").forEach(b => {
    b.addEventListener("click", () => {
      const id = b.getAttribute("data-minus");
      const item = state.cart.find(x => x.id === id);
      if (!item) return;
      updateQty(id, item.qty - 1);
    });
  });

  els.cartList.querySelectorAll("[data-plus]").forEach(b => {
    b.addEventListener("click", () => {
      const id = b.getAttribute("data-plus");
      const item = state.cart.find(x => x.id === id);
      if (!item) return;
      updateQty(id, item.qty + 1);
    });
  });

  els.cartList.querySelectorAll("[data-remove]").forEach(b => {
    b.addEventListener("click", () => {
      removeFromCart(b.getAttribute("data-remove"));
      showToast("Đã xoá khỏi giỏ");
    });
  });

  els.cartList.querySelectorAll("[data-qty]").forEach(inp => {
    inp.addEventListener("change", () => {
      const id = inp.getAttribute("data-qty");
      const v = parseInt(inp.value, 10);
      if (Number.isNaN(v)) return;
      updateQty(id, v);
    });
  });
}

function openCart() {
  els.cartModal.classList.add("is-open");
  els.cartModal.setAttribute("aria-hidden", "false");
  document.body.style.overflow = "hidden";
}

function closeCart() {
  els.cartModal.classList.remove("is-open");
  els.cartModal.setAttribute("aria-hidden", "true");
  document.body.style.overflow = "";
}

function checkout() {
  const total = formatVND(cartTotal());
  const lines = state.cart.map(it => {
    const p = getProduct(it.id);
    const name = p ? p.name : it.id;
    const line = p ? p.price * it.qty : 0;
    return `- ${name} x${it.qty} = ${formatVND(line)}`;
  }).join("\n");

  const msg =
`ĐƠN HÀNG PANNA COTTA
${lines}

Tổng: ${total}

Vui lòng liên hệ shop để xác nhận (địa chỉ + thời gian giao).
`;

  // Demo: mở prompt bằng alert là không khuyến khích; dùng toast + copy qua clipboard nếu có
  if (navigator.clipboard?.writeText) {
    navigator.clipboard.writeText(msg)
      .then(() => showToast("Đã copy nội dung đơn hàng!"))
      .catch(() => showToast("Không copy được, bạn có thể tự chọn nội dung."));
  } else {
    showToast("Trình duyệt chưa hỗ trợ copy. Vui lòng copy thủ công từ ô bên dưới.");
  }

  els.checkoutHint.textContent = "Đã tạo nội dung đơn hàng (để copy/nhắn shop).";
  // Hiển thị tạm trong hint bằng cách set text (gọn lại)
  els.checkoutHint.textContent = "Đã sẵn sàng nội dung đơn hàng. Hãy dán vào Zalo/WhatsApp để gửi shop (demo).";
}

function init() {
  els.year.textContent = new Date().getFullYear();

  // Filter buttons
  els.productFilterBtns.forEach(btn => {
    btn.addEventListener("click", () => {
      els.productFilterBtns.forEach(x => x.classList.remove("is-active"));
      btn.classList.add("is-active");
      state.filter = btn.getAttribute("data-filter");
      renderProducts();
    });
  });

  // Render initial products + cart
  renderProducts();
  renderCart();

  // Cart modal controls
  els.openCartBtn.addEventListener("click", openCart);
  els.closeCartBtn.addEventListener("click", closeCart);
  els.cartBackdrop.addEventListener("click", closeCart);

  els.clearCartBtn.addEventListener("click", () => {
    state.cart = [];
    renderCart();
    showToast("Đã xoá giỏ hàng");
  });

  els.checkoutBtn.addEventListener("click", checkout);

  // Contact form (demo)
  els.contactForm.addEventListener("submit", (e) => {
    e.preventDefault();
    showToast("Cảm ơn bạn! (Demo) Mình sẽ phản hồi sớm.");
    els.contactForm.reset();
  });
}

init();
