{% extends "base.html" %}
{% block title %}Kelola Add-ons — Borcelle Coffee Admin{% endblock %}

{% block navbar %}{% include "admin/sidebar.html" %}{% endblock %}

{% block content %}
<div class="lg:ml-64 p-4 md:p-6 bg-gray-50 min-h-screen">
  <div class="flex items-center justify-between mb-2">
    <h1 class="text-2xl font-extrabold text-charcoal">➕ Kelola Add-ons</h1>
    <button onclick="openAddonModal()"
            class="bg-primary-500 hover:bg-primary-600 text-white font-bold px-5 py-2.5 rounded-xl transition flex items-center gap-2 text-sm shadow-md">
      <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
      Tambah Add-on
    </button>
  </div>
  <p class="text-gray-500 text-sm mb-4">Add-on adalah tambahan kustomisasi yang bisa dipilih pelanggan di halaman menu, contoh: Extra Shot, Oat Milk, Whipped Cream, Boba.</p>

  <!-- Flash messages -->
  {% with messages = get_flashed_messages(with_categories=true) %}
    {% if messages %}
      {% for cat, msg in messages %}
      <div class="mb-4 px-5 py-3 rounded-xl text-sm font-semibold
        {% if cat == 'success' %}bg-green-100 text-green-800 border border-green-200
        {% elif cat == 'error' %}bg-red-100 text-red-800 border border-red-200
        {% else %}bg-blue-100 text-blue-800 border border-blue-200{% endif %}">
        {{ '✅' if cat == 'success' else ('❌' if cat == 'error' else 'ℹ️') }} {{ msg }}
      </div>
      {% endfor %}
    {% endif %}
  {% endwith %}

  <!-- Tabel Add-ons -->
  <div class="bg-white rounded-2xl shadow-sm overflow-hidden">
    <table class="w-full text-sm">
      <thead class="bg-gray-50 border-b border-gray-100">
        <tr>
          <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500 uppercase tracking-wide">Nama Add-on</th>
          <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500 uppercase tracking-wide">Berlaku Untuk</th>
          <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500 uppercase tracking-wide">Harga Tambahan</th>
          <th class="px-4 py-3 text-center text-xs font-semibold text-gray-500 uppercase tracking-wide">Status</th>
          <th class="px-4 py-3 text-right text-xs font-semibold text-gray-500 uppercase tracking-wide">Aksi</th>
        </tr>
      </thead>
      <tbody class="divide-y divide-gray-50">
        {% set kategori_labels = {'semua': '🌐 Semua Kategori', 'kopi': '☕ Kopi', 'non_kopi': '🍵 Non-Kopi', 'snack': '🥐 Snack', 'makanan': '🥪 Makanan'} %}
        {% for a in addons %}
        <tr class="hover:bg-gray-50 transition">
          <td class="px-5 py-3.5 font-semibold text-charcoal">{{ a.nama }}</td>
          <td class="px-4 py-3.5">
            <span class="bg-gray-100 text-gray-600 text-xs font-medium px-2.5 py-1 rounded-full">{{ kategori_labels.get(a.kategori, a.kategori) }}</span>
          </td>
          <td class="px-4 py-3.5 font-semibold text-charcoal">+Rp {{ '{:,}'.format(a.harga_tambahan).replace(',','.') }}</td>
          <td class="px-4 py-3.5 text-center">
            {% if a.aktif %}
            <span class="bg-green-100 text-green-700 text-xs font-bold px-3 py-1.5 rounded-full">✅ Aktif</span>
            {% else %}
            <span class="badge-habis text-xs font-bold px-3 py-1.5 rounded-full">❌ Nonaktif</span>
            {% endif %}
          </td>
          <td class="px-4 py-3.5 text-right">
            <div class="flex items-center justify-end gap-2">
              <button onclick='editAddonFromEl(this)'
                      data-id="{{ a.id }}" data-nama="{{ a.nama }}" data-harga="{{ a.harga_tambahan }}"
                      data-kategori="{{ a.kategori }}" data-aktif="{{ '1' if a.aktif else '0' }}"
                      class="text-blue-600 hover:bg-blue-50 p-1.5 rounded-lg transition" aria-label="Edit">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/></svg>
              </button>
              <form method="POST" action="/admin/addons/{{ a.id }}/hapus" onsubmit="return confirm('Hapus add-on \'{{ a.nama }}\'?')" class="inline">
                <button type="submit" class="text-red-500 hover:bg-red-50 p-1.5 rounded-lg transition" aria-label="Hapus">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg>
                </button>
              </form>
            </div>
          </td>
        </tr>
        {% else %}
        <tr><td colspan="5" class="px-5 py-8 text-center text-gray-400">Belum ada add-on. Tambahkan yang pertama!</td></tr>
        {% endfor %}
      </tbody>
    </table>
  </div>
</div>

<!-- Modal Tambah/Edit Add-on -->
<div id="modal-addon" class="modal-overlay fixed inset-0 bg-black/50 z-50 hidden flex items-center justify-center p-4">
  <div class="modal-box bg-white w-full max-w-md rounded-3xl overflow-hidden scale-95 opacity-0 transition-all">
    <div class="flex items-center justify-between px-6 py-4 border-b border-gray-100">
      <h3 id="modal-addon-title" class="font-extrabold text-charcoal text-lg">Tambah Add-on</h3>
      <button onclick="closeModal('modal-addon')" class="text-gray-400 hover:text-gray-600 transition text-xl">✕</button>
    </div>
    <form id="form-addon" method="POST" action="/admin/addons/tambah" class="p-6 space-y-4">
      <div>
        <label class="block text-sm font-semibold text-gray-700 mb-1.5">Nama Add-on <span class="text-red-500">*</span></label>
        <input type="text" id="addon-nama" name="nama" required placeholder="Contoh: Extra Shot Espresso"
               class="w-full border border-gray-200 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-primary-300"/>
      </div>
      <div class="grid grid-cols-2 gap-4">
        <div>
          <label class="block text-sm font-semibold text-gray-700 mb-1.5">Harga Tambahan (Rp)</label>
          <input type="number" id="addon-harga" name="harga_tambahan" min="0" placeholder="0" required
                 class="w-full border border-gray-200 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-primary-300"/>
        </div>
        <div>
          <label class="block text-sm font-semibold text-gray-700 mb-1.5">Berlaku Untuk</label>
          <select id="addon-kategori" name="kategori" class="w-full border border-gray-200 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-primary-300 bg-white">
            <option value="semua">🌐 Semua Kategori</option>
            <option value="kopi">☕ Kopi</option>
            <option value="non_kopi">🍵 Non-Kopi</option>
            <option value="snack">🥐 Snack</option>
            <option value="makanan">🥪 Makanan</option>
          </select>
        </div>
      </div>
      <div>
        <label class="flex items-center gap-2 text-sm font-semibold text-gray-700">
          <input type="checkbox" id="addon-aktif" name="aktif" value="1" checked class="rounded border-gray-300 text-primary-500 focus:ring-primary-300"/>
          Aktif (tampil di menu pelanggan)
        </label>
      </div>
      <div class="flex gap-3 pt-2">
        <button type="button" onclick="closeModal('modal-addon')"
                class="flex-1 border border-gray-200 text-gray-600 font-semibold py-3 rounded-xl hover:bg-gray-50 transition text-sm">Batal</button>
        <button type="submit"
                class="flex-1 bg-primary-500 hover:bg-primary-600 text-white font-bold py-3 rounded-xl transition text-sm shadow-md">Simpan</button>
      </div>
    </form>
  </div>
</div>
{% endblock %}

{% block extra_scripts %}
<script>
function openAddonModal() {
  document.getElementById('modal-addon-title').textContent = 'Tambah Add-on';
  document.getElementById('form-addon').reset();
  document.getElementById('form-addon').action = '/admin/addons/tambah';
  document.getElementById('addon-aktif').checked = true;
  openModal('modal-addon');
}

function editAddonFromEl(btn) {
  const d = btn.dataset;
  document.getElementById('modal-addon-title').textContent = 'Edit Add-on';
  document.getElementById('form-addon').action = `/admin/addons/${d.id}/edit`;
  document.getElementById('addon-nama').value = d.nama;
  document.getElementById('addon-harga').value = d.harga;
  document.getElementById('addon-kategori').value = d.kategori;
  document.getElementById('addon-aktif').checked = d.aktif === '1';
  openModal('modal-addon');
}
</script>
{% endblock %}
