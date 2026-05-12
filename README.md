# -
// 網頁開啟時自動讀取舊資料
window.onload = function() {
    const savedData = JSON.parse(localStorage.getItem('foodList')) || [];
    savedData.forEach(item => {
        createCard(item.name, item.address, item.yue, item.liao, item.imageUrl, false);
    });
};

function addStore() {
    const name = document.getElementById('storeName').value;
    const address = document.getElementById('address').value;
    const yue = document.getElementById('yueScore').value;
    const liao = document.getElementById('liaoScore').value;
    const imageInput = document.getElementById('imageInput');

    if (!name || !address) {
        alert("記得填寫店名跟地址喔！");
        return;
    }

    const reader = new FileReader();
    reader.onload = function(e) {
        const imageUrl = e.target.result || 'https://via.placeholder.com/300x200?text=No+Image';
        createCard(name, address, yue, liao, imageUrl, true);
    };

    if (imageInput.files[0]) {
        reader.readAsDataURL(imageInput.files[0]);
    } else {
        createCard(name, address, yue, liao, 'https://via.placeholder.com/300x200?text=No+Image', true);
    }

    document.getElementById('storeName').value = '';
    document.getElementById('address').value = '';
}

function createCard(name, address, yue, liao, imageUrl, isNew) {
    const container = document.getElementById('listContainer');
    const card = document.createElement('div');
    card.className = 'store-card';
    card.innerHTML = `
        <img src="${imageUrl}" alt="美食照片">
        <div class="store-info">
            <h3>${name}</h3>
            <p>📍 ${address}</p>
        </div>
        <div class="rating-box">
            <div class="rating-item"><span>小玥</span><div class="rating-score">${yue}</div></div>
            <div class="rating-item"><span>廖鑫鑫</span><div class="rating-score">${liao}</div></div>
        </div>
    `;
    container.prepend(card);

    // 如果是新加的，就存進瀏覽器記憶體
    if (isNew) {
        const savedData = JSON.parse(localStorage.getItem('foodList')) || [];
        savedData.push({name, address, yue, liao, imageUrl});
        localStorage.setItem('foodList', JSON.stringify(savedData));
    }
}
