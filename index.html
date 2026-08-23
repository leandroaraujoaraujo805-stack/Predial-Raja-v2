import express from 'express';
import multer from 'multer';
import Database from 'better-sqlite3';
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';
import path from 'path';
import fs from 'fs';
import { fileURLToPath } from 'url';
import crypto from 'crypto';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const app = express();

const PORT = process.env.PORT || 3000;
const SECRET =
  process.env.JWT_SECRET ||
  'TROQUE-ESTA-CHAVE-EM-PRODUCAO';

const dataDir = path.join(
  __dirname,
  'server',
  'data'
);

const uploadDir = path.join(
  dataDir,
  'uploads'
);

fs.mkdirSync(
  dataDir,
  { recursive: true }
);

fs.mkdirSync(
  uploadDir,
  { recursive: true }
);

const db = new Database(
  path.join(
    dataDir,
    'predial_raja.db'
  )
);

const upload = multer({
  dest: uploadDir,
  limits: {
    fileSize: 15 * 1024 * 1024
  }
});

app.use(
  express.json({
    limit: '4mb'
  })
);

/*
  IMPORTANTE:
  O index.html está na raiz do projeto.
*/
app.use(
  express.static(__dirname)
);

app.use(
  '/uploads',
  express.static(uploadDir)
);

db.pragma(
  'foreign_keys = ON'
);

db.exec(`
CREATE TABLE IF NOT EXISTS condominiums(
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  cnpj TEXT,
  address TEXT,
  phone TEXT,
  email TEXT,
  logo TEXT,
  created_at TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS users(
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  phone TEXT,
  active INTEGER NOT NULL DEFAULT 1,
  deleted_at TEXT,
  created_at TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS memberships(
  id TEXT PRIMARY KEY,
  user_id TEXT NOT NULL,
  condominium_id TEXT NOT NULL,
  role TEXT NOT NULL,
  job_title TEXT,
  apt TEXT,
  permissions TEXT NOT NULL DEFAULT '{}'
);

CREATE TABLE IF NOT EXISTS groups(
  id TEXT PRIMARY KEY,
  condominium_id TEXT NOT NULL,
  name TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS group_members(
  group_id TEXT NOT NULL,
  user_id TEXT NOT NULL,
  PRIMARY KEY(group_id,user_id)
);

CREATE TABLE IF NOT EXISTS posts(
  id TEXT PRIMARY KEY,
  condominium_id TEXT NOT NULL,
  author_id TEXT NOT NULL,
  title TEXT NOT NULL,
  body TEXT NOT NULL,
  category TEXT,
  created_at TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS requests(
  id TEXT PRIMARY KEY,
  condominium_id TEXT NOT NULL,
  requester_id TEXT NOT NULL,
  title TEXT NOT NULL,
  description TEXT NOT NULL,
  status TEXT NOT NULL,
  created_at TEXT NOT NULL,
  updated_at TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS documents(
  id TEXT PRIMARY KEY,
  condominium_id TEXT NOT NULL,
  uploaded_by TEXT NOT NULL,
  title TEXT NOT NULL,
  file_path TEXT NOT NULL,
  original_name TEXT NOT NULL,
  mime TEXT,
  visibility TEXT NOT NULL DEFAULT 'all',
  created_at TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS financial_months(
  id TEXT PRIMARY KEY,
  condominium_id TEXT NOT NULL,
  reference_month TEXT NOT NULL,
  expected_revenue REAL NOT NULL DEFAULT 0,
  received_revenue REAL NOT NULL DEFAULT 0,
  expenses REAL NOT NULL DEFAULT 0,
  total_units INTEGER NOT NULL DEFAULT 0,
  delinquent_units INTEGER NOT NULL DEFAULT 0,
  created_at TEXT NOT NULL,
  UNIQUE(condominium_id,reference_month)
);

CREATE TABLE IF NOT EXISTS messages(
  id TEXT PRIMARY KEY,
  condominium_id TEXT NOT NULL,
  sender_id TEXT NOT NULL,
  target_type TEXT NOT NULL,
  target_id TEXT NOT NULL,
  body TEXT NOT NULL,
  created_at TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS audit_logs(
  id TEXT PRIMARY KEY,
  condominium_id TEXT,
  user_id TEXT,
  action TEXT NOT NULL,
  entity_type TEXT,
  entity_id TEXT,
  details TEXT,
  created_at TEXT NOT NULL
);
`);

const id = () =>
  crypto.randomUUID();

const now = () =>
  new Date().toISOString();

function ensureColumn(
  table,
  column,
  definition
) {
  const cols = db
    .prepare(
      `PRAGMA table_info(${table})`
    )
    .all()
    .map(
      x => x.name
    );

  if (!cols.includes(column)) {
    db.exec(
      `ALTER TABLE ${table} ADD COLUMN ${definition}`
    );
  }
}

ensureColumn(
  'users',
  'deleted_at',
  'deleted_at TEXT'
);

ensureColumn(
  'documents',
  'visibility',
  "visibility TEXT NOT NULL DEFAULT 'all'"
);

function audit(
  condoId,
  userId,
  action,
  entityType = '',
  entityId = '',
  details = {}
) {
  db.prepare(`
    INSERT INTO audit_logs
    VALUES (?,?,?,?,?,?,?,?)
  `).run(
    id(),
    condoId || null,
    userId || null,
    action,
    entityType,
    entityId,
    JSON.stringify(details),
    now()
  );
}

function seed() {
  const count = db
    .prepare(
      'SELECT COUNT(*) c FROM condominiums'
    )
    .get()
    .c;

  if (count) return;

  const condoId = id();
  const adminId = id();
  const sindicoId = id();

  const pw =
    bcrypt.hashSync(
      '123456',
      10
    );

  db.prepare(`
    INSERT INTO condominiums
    VALUES (?,?,?,?,?,?,?,?)
  `).run(
    condoId,
    "Condomínio do Edifício D'orvilliers",
    '',
    '',
    '',
    '',
    '',
    now()
  );

  db.prepare(`
    INSERT INTO users(
      id,
      name,
      email,
      password_hash,
      phone,
      active,
      deleted_at,
      created_at
    )
    VALUES (?,?,?,?,?,?,?,?)
  `).run(
    adminId,
    'Administrador Predial Raja',
    'admin@predialraja.local',
    pw,
    '',
    1,
    null,
    now()
  );

  db.prepare(`
    INSERT INTO users(
      id,
      name,
      email,
      password_hash,
      phone,
      active,
      deleted_at,
      created_at
    )
    VALUES (?,?,?,?,?,?,?,?)
  `).run(
    sindicoId,
    'Leandro Araújo',
    'sindico@dorvilliers.local',
    pw,
    '',
    1,
    null,
    now()
  );

  db.prepare(`
    INSERT INTO memberships
    VALUES (?,?,?,?,?,?,?)
  `).run(
    id(),
    adminId,
    condoId,
    'admin',
    '',
    '',
    '{}'
  );

  db.prepare(`
    INSERT INTO memberships
    VALUES (?,?,?,?,?,?,?)
  `).run(
    id(),
    sindicoId,
    condoId,
    'sindico',
    '',
    '',
    '{}'
  );

  for (
    const n of [
      'Sra. Lina',
      'Dr. Carlos',
      'Sra. Jane'
    ]
  ) {
    const uid = id();

    db.prepare(`
      INSERT INTO users(
        id,
        name,
        email,
        password_hash,
        phone,
        active,
        deleted_at,
        created_at
      )
      VALUES (?,?,?,?,?,?,?,?)
    `).run(
      uid,
      n,
      `${uid}@local`,
      pw,
      '',
      1,
      null,
      now()
    );

    db.prepare(`
      INSERT INTO memberships
      VALUES (?,?,?,?,?,?,?)
    `).run(
      id(),
      uid,
      condoId,
      'conselho',
      '',
      '',
      '{}'
    );
  }

  for (
    let i = 1;
    i <= 4;
    i++
  ) {
    const uid = id();

    db.prepare(`
      INSERT INTO users(
        id,
        name,
        email,
        password_hash,
        phone,
        active,
        deleted_at,
        created_at
      )
      VALUES (?,?,?,?,?,?,?,?)
    `).run(
      uid,
      `Porteiro ${i}`,
      `${uid}@local`,
      pw,
      '',
      1,
      null,
      now()
    );

    db.prepare(`
      INSERT INTO memberships
      VALUES (?,?,?,?,?,?,?)
    `).run(
      id(),
      uid,
      condoId,
      'funcionario',
      'Porteiro 12x36',
      '',
      JSON.stringify({
        mural: true,
        chat: true,
        solicitacoes: true
      })
    );
  }
}

seed();

function auth(
  req,
  res,
  next
) {
  const h =
    req.headers.authorization || '';

  const token =
    h.startsWith('Bearer ')
      ? h.slice(7)
      : null;

  if (!token) {
    return res
      .status(401)
      .json({
        error:
          'Não autenticado'
      });
  }

  try {
    const decoded =
      jwt.verify(
        token,
        SECRET
      );

    let u = db
      .prepare(`
        SELECT
          id,
          name,
          email,
          active,
          deleted_at
        FROM users
        WHERE id=?
      `)
      .get(
        decoded.id
      );

    if (
      !u &&
      decoded.email
    ) {
      u = db
        .prepare(`
          SELECT
            id,
            name,
            email,
            active,
            deleted_at
          FROM users
          WHERE email=?
        `)
        .get(
          decoded.email
        );
    }

    if (
      !u ||
      !u.active ||
      u.deleted_at
    ) {
      return res
        .status(401)
        .json({
          error:
            'Sessão expirada'
        });
    }

    req.user = {
      id: u.id,
      name: u.name,
      email: u.email
    };

    next();

  } catch {
    return res
      .status(401)
      .json({
        error:
          'Sessão inválida'
      });
  }
}

function member(
  uid,
  cid
) {
  return db
    .prepare(`
      SELECT *
      FROM memberships
      WHERE user_id=?
      AND condominium_id=?
    `)
    .get(
      uid,
      cid
    );
}

function canManage(m) {
  return (
    m &&
    [
      'admin',
      'sindico'
    ].includes(
      m.role
    )
  );
}

function parsePermissions(value) {
  try {
    return JSON.parse(
      value || '{}'
    );
  } catch {
    return {};
  }
}

/* LOGIN */

app.post(
  '/api/login',
  (req, res) => {

    const {
      email,
      password
    } =
      req.body || {};

    const u = db
      .prepare(`
        SELECT *
        FROM users
        WHERE email=?
        AND active=1
        AND deleted_at IS NULL
      `)
      .get(email);

    if (
      !u ||
      !bcrypt.compareSync(
        password || '',
        u.password_hash
      )
    ) {
      return res
        .status(401)
        .json({
          error:
            'E-mail ou senha inválidos'
        });
    }

    const token =
      jwt.sign(
        {
          id: u.id,
          name: u.name,
          email: u.email
        },
        SECRET,
        {
          expiresIn: '12h'
        }
      );

    res.json({
      token,
      user: {
        id: u.id,
        name: u.name,
        email: u.email
      }
    });
  }
);

/* ME */

app.get(
  '/api/me',
  auth,
  (req, res) => {

    const memberships =
      db.prepare(`
        SELECT
          m.*,
          c.name condominium_name
        FROM memberships m
        JOIN condominiums c
        ON c.id=m.condominium_id
        WHERE m.user_id=?
      `).all(
        req.user.id
      );

    res.json({
      user: req.user,
      memberships
    });
  }
);

/* CONDOMINIOS */

app.get(
  '/api/condominiums',
  auth,
  (req, res) => {

    const rows =
      db.prepare(`
        SELECT
          c.*,
          m.role,
          m.job_title,
          m.apt
        FROM memberships m
        JOIN condominiums c
        ON c.id=m.condominium_id
        WHERE m.user_id=?
      `).all(
        req.user.id
      );

    res.json(rows);
  }
);

app.post(
  '/api/condominiums',
  auth,
  (req, res) => {

    const anyAdmin =
      db.prepare(`
        SELECT 1
        FROM memberships
        WHERE user_id=?
        AND role='admin'
        LIMIT 1
      `).get(
        req.user.id
      );

    const isMaster =
      req.user.email ===
      'admin@predialraja.local';

    if (
      !anyAdmin &&
      !isMaster
    ) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const name =
      String(
        req.body?.name || ''
      ).trim();

    if (!name) {
      return res
        .status(400)
        .json({
          error:
            'Nome obrigatório'
        });
    }

    const c = {
      id: id(),
      name,
      cnpj: '',
      address: '',
      phone: '',
      email: '',
      logo: '',
      created_at: now()
    };

    db.prepare(`
      INSERT INTO condominiums
      VALUES (?,?,?,?,?,?,?,?)
    `).run(
      c.id,
      c.name,
      c.cnpj,
      c.address,
      c.phone,
      c.email,
      c.logo,
      c.created_at
    );

    db.prepare(`
      INSERT INTO memberships
      VALUES (?,?,?,?,?,?,?)
    `).run(
      id(),
      req.user.id,
      c.id,
      'admin',
      '',
      '',
      '{}'
    );

    audit(
      c.id,
      req.user.id,
      'CREATE_CONDOMINIUM',
      'condominium',
      c.id,
      {
        name
      }
    );

    res.json(c);
  }
);

app.put(
  '/api/condominiums/:id',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.id
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const b =
      req.body || {};

    db.prepare(`
      UPDATE condominiums
      SET
        name=?,
        cnpj=?,
        address=?,
        phone=?,
        email=?,
        logo=?
      WHERE id=?
    `).run(
      b.name || '',
      b.cnpj || '',
      b.address || '',
      b.phone || '',
      b.email || '',
      b.logo || '',
      req.params.id
    );

    audit(
      req.params.id,
      req.user.id,
      'UPDATE_CONDOMINIUM',
      'condominium',
      req.params.id,
      b
    );

    res.json({
      ok: true
    });
  }
);

/* PESSOAS */

app.get(
  '/api/people/:condo',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!m) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    const rows =
      db.prepare(`
        SELECT
          u.id,
          u.name,
          u.email,
          u.phone,
          u.active,
          u.deleted_at,
          m.role,
          m.job_title,
          m.apt,
          m.permissions
        FROM memberships m
        JOIN users u
        ON u.id=m.user_id
        WHERE m.condominium_id=?
        AND u.deleted_at IS NULL
        ORDER BY u.name
      `).all(
        req.params.condo
      );

    res.json(
      rows.map(
        x => ({
          ...x,
          permissions:
            parsePermissions(
              x.permissions
            )
        })
      )
    );
  }
);

app.post(
  '/api/people/:condo',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const {
      name,
      email,
      password,
      role,
      job_title = '',
      apt = '',
      phone = ''
    } =
      req.body || {};

    if (
      !name ||
      !email ||
      !password ||
      !role
    ) {
      return res
        .status(400)
        .json({
          error:
            'Campos obrigatórios'
        });
    }

    const uid = id();

    try {

      db.prepare(`
        INSERT INTO users(
          id,
          name,
          email,
          password_hash,
          phone,
          active,
          deleted_at,
          created_at
        )
        VALUES (?,?,?,?,?,?,?,?)
      `).run(
        uid,
        name,
        email,
        bcrypt.hashSync(
          password,
          10
        ),
        phone,
        1,
        null,
        now()
      );

      db.prepare(`
        INSERT INTO memberships
        VALUES (?,?,?,?,?,?,?)
      `).run(
        id(),
        uid,
        req.params.condo,
        role,
        job_title,
        apt,
        '{}'
      );

      audit(
        req.params.condo,
        req.user.id,
        'CREATE_USER',
        'user',
        uid,
        {
          name,
          email,
          role
        }
      );

      res.json({
        id: uid
      });

    } catch {
      return res
        .status(400)
        .json({
          error:
            'E-mail já cadastrado ou dados inválidos'
        });
    }
  }
);

app.put(
  '/api/people/:condo/:id',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const b =
      req.body || {};

    db.prepare(`
      UPDATE users
      SET
        name=?,
        phone=?
      WHERE id=?
    `).run(
      b.name || '',
      b.phone || '',
      req.params.id
    );

    db.prepare(`
      UPDATE memberships
      SET
        role=?,
        job_title=?,
        apt=?
      WHERE user_id=?
      AND condominium_id=?
    `).run(
      b.role || 'condomino',
      b.job_title || '',
      b.apt || '',
      req.params.id,
      req.params.condo
    );

    audit(
      req.params.condo,
      req.user.id,
      'UPDATE_USER',
      'user',
      req.params.id,
      b
    );

    res.json({
      ok: true
    });
  }
);

app.patch(
  '/api/people/:condo/:id',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const active =
      req.body?.active
        ? 1
        : 0;

    db.prepare(`
      UPDATE users
      SET active=?
      WHERE id=?
    `).run(
      active,
      req.params.id
    );

    audit(
      req.params.condo,
      req.user.id,
      active
        ? 'REACTIVATE_USER'
        : 'SUSPEND_USER',
      'user',
      req.params.id,
      {}
    );

    res.json({
      ok: true
    });
  }
);

app.delete(
  '/api/people/:condo/:id',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    if (
      req.params.id ===
      req.user.id
    ) {
      return res
        .status(400)
        .json({
          error:
            'Você não pode excluir seu próprio usuário'
        });
    }

    db.prepare(`
      UPDATE users
      SET
        active=0,
        deleted_at=?
      WHERE id=?
    `).run(
      now(),
      req.params.id
    );

    audit(
      req.params.condo,
      req.user.id,
      'DELETE_USER',
      'user',
      req.params.id,
      {}
    );

    res.json({
      ok: true
    });
  }
);

app.put(
  '/api/people/:condo/:id/permissions',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const permissions =
      req.body?.permissions || {};

    db.prepare(`
      UPDATE memberships
      SET permissions=?
      WHERE user_id=?
      AND condominium_id=?
    `).run(
      JSON.stringify(
        permissions
      ),
      req.params.id,
      req.params.condo
    );

    audit(
      req.params.condo,
      req.user.id,
      'UPDATE_PERMISSIONS',
      'user',
      req.params.id,
      permissions
    );

    res.json({
      ok: true
    });
  }
);

/* GRUPOS */

app.get(
  '/api/groups/:condo',
  auth,
  (req, res) => {

    if (
      !member(
        req.user.id,
        req.params.condo
      )
    ) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    const gs =
      db.prepare(`
        SELECT *
        FROM groups
        WHERE condominium_id=?
      `).all(
        req.params.condo
      );

    for (
      const g of gs
    ) {
      g.members =
        db.prepare(`
          SELECT user_id
          FROM group_members
          WHERE group_id=?
        `).all(
          g.id
        ).map(
          x => x.user_id
        );
    }

    res.json(gs);
  }
);

app.post(
  '/api/groups/:condo',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const gid = id();

    db.prepare(`
      INSERT INTO groups
      VALUES (?,?,?)
    `).run(
      gid,
      req.params.condo,
      req.body.name
    );

    for (
      const uid of
      req.body.members || []
    ) {
      db.prepare(`
        INSERT OR IGNORE
        INTO group_members
        VALUES (?,?)
      `).run(
        gid,
        uid
      );
    }

    res.json({
      id: gid
    });
  }
);

/* COMUNICADOS */

app.get(
  '/api/posts/:condo',
  auth,
  (req, res) => {

    if (
      !member(
        req.user.id,
        req.params.condo
      )
    ) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    res.json(
      db.prepare(`
        SELECT
          p.*,
          u.name author_name
        FROM posts p
        JOIN users u
        ON u.id=p.author_id
        WHERE condominium_id=?
        ORDER BY created_at DESC
      `).all(
        req.params.condo
      )
    );
  }
);

app.post(
  '/api/posts/:condo',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    db.prepare(`
      INSERT INTO posts
      VALUES (?,?,?,?,?,?,?)
    `).run(
      id(),
      req.params.condo,
      req.user.id,
      req.body.title,
      req.body.body,
      req.body.category ||
        'Comunicado',
      now()
    );

    res.json({
      ok: true
    });
  }
);

/* SOLICITACOES */

app.get(
  '/api/requests/:condo',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!m) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    const q =
      canManage(m)
        ? `
          SELECT
            r.*,
            u.name requester_name
          FROM requests r
          JOIN users u
          ON u.id=r.requester_id
          WHERE condominium_id=?
          ORDER BY created_at DESC
        `
        : `
          SELECT
            r.*,
            u.name requester_name
          FROM requests r
          JOIN users u
          ON u.id=r.requester_id
          WHERE condominium_id=?
          AND requester_id=?
          ORDER BY created_at DESC
        `;

    res.json(
      canManage(m)
        ? db.prepare(q)
            .all(
              req.params.condo
            )
        : db.prepare(q)
            .all(
              req.params.condo,
              req.user.id
            )
    );
  }
);

app.post(
  '/api/requests/:condo',
  auth,
  (req, res) => {

    if (
      !member(
        req.user.id,
        req.params.condo
      )
    ) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    db.prepare(`
      INSERT INTO requests
      VALUES (?,?,?,?,?,?,?,?)
    `).run(
      id(),
      req.params.condo,
      req.user.id,
      req.body.title,
      req.body.description,
      'Aberto',
      now(),
      now()
    );

    res.json({
      ok: true
    });
  }
);

app.put(
  '/api/requests/:condo/:id',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    db.prepare(`
      UPDATE requests
      SET
        status=?,
        updated_at=?
      WHERE id=?
      AND condominium_id=?
    `).run(
      req.body.status,
      now(),
      req.params.id,
      req.params.condo
    );

    res.json({
      ok: true
    });
  }
);

/* DOCUMENTOS */

app.get(
  '/api/documents/:condo',
  auth,
  (req, res) => {

    if (
      !member(
        req.user.id,
        req.params.condo
      )
    ) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    res.json(
      db.prepare(`
        SELECT *
        FROM documents
        WHERE condominium_id=?
        ORDER BY created_at DESC
      `).all(
        req.params.condo
      )
    );
  }
);

app.post(
  '/api/documents/:condo',
  auth,
  upload.single('file'),
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    if (!req.file) {
      return res
        .status(400)
        .json({
          error:
            'Arquivo ausente'
        });
    }

    const did = id();

    const target =
      `${did}_${
        req.file.originalname
          .replace(
            /[^a-zA-Z0-9._-]/g,
            '_'
          )
      }`;

    fs.renameSync(
      req.file.path,
      path.join(
        uploadDir,
        target
      )
    );

    db.prepare(`
      INSERT INTO documents(
        id,
        condominium_id,
        uploaded_by,
        title,
        file_path,
        original_name,
        mime,
        visibility,
        created_at
      )
      VALUES (?,?,?,?,?,?,?,?,?)
    `).run(
      did,
      req.params.condo,
      req.user.id,
      req.body.title ||
        req.file.originalname,
      target,
      req.file.originalname,
      req.file.mimetype,
      req.body.visibility ||
        'all',
      now()
    );

    res.json({
      ok: true
    });
  }
);

/* MENSAGENS */

app.get(
  '/api/messages/:condo',
  auth,
  (req, res) => {

    if (
      !member(
        req.user.id,
        req.params.condo
      )
    ) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    const {
      target_type,
      target_id
    } =
      req.query;

    if (
      target_type ===
      'user'
    ) {
      return res.json(
        db.prepare(`
          SELECT *
          FROM messages
          WHERE condominium_id=?
          AND target_type='user'
          AND (
            (sender_id=? AND target_id=?)
            OR
            (sender_id=? AND target_id=?)
          )
          ORDER BY created_at
        `).all(
          req.params.condo,
          req.user.id,
          target_id,
          target_id,
          req.user.id
        )
      );
    }

    const inGroup =
      db.prepare(`
        SELECT 1
        FROM group_members
        WHERE group_id=?
        AND user_id=?
      `).get(
        target_id,
        req.user.id
      );

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (
      !inGroup &&
      !canManage(m)
    ) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso ao grupo'
        });
    }

    res.json(
      db.prepare(`
        SELECT *
        FROM messages
        WHERE condominium_id=?
        AND target_type='group'
        AND target_id=?
        ORDER BY created_at
      `).all(
        req.params.condo,
        target_id
      )
    );
  }
);

app.post(
  '/api/messages/:condo',
  auth,
  (req, res) => {

    if (
      !member(
        req.user.id,
        req.params.condo
      )
    ) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    const {
      target_type,
      target_id,
      body
    } =
      req.body;

    db.prepare(`
      INSERT INTO messages
      VALUES (?,?,?,?,?,?,?)
    `).run(
      id(),
      req.params.condo,
      req.user.id,
      target_type,
      target_id,
      body,
      now()
    );

    res.json({
      ok: true
    });
  }
);

/* FINANCEIRO */

app.get(
  '/api/finance/:condo',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!m) {
      return res
        .status(403)
        .json({
          error:
            'Sem acesso'
        });
    }

    const rows =
      db.prepare(`
        SELECT *
        FROM financial_months
        WHERE condominium_id=?
        ORDER BY reference_month
      `).all(
        req.params.condo
      );

    res.json(
      rows.map(
        r => ({
          ...r,

          delinquency_rate:
            r.total_units
              ? +(
                  r.delinquent_units /
                  r.total_units *
                  100
                ).toFixed(2)
              : 0,

          collection_rate:
            r.expected_revenue
              ? +(
                  r.received_revenue /
                  r.expected_revenue *
                  100
                ).toFixed(2)
              : 0,

          balance:
            +(
              r.received_revenue -
              r.expenses
            ).toFixed(2)
        })
      )
    );
  }
);

app.post(
  '/api/finance/:condo',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const b =
      req.body || {};

    if (
      !b.reference_month
    ) {
      return res
        .status(400)
        .json({
          error:
            'Mês obrigatório'
        });
    }

    const ex =
      db.prepare(`
        SELECT id
        FROM financial_months
        WHERE condominium_id=?
        AND reference_month=?
      `).get(
        req.params.condo,
        b.reference_month
      );

    if (ex) {

      db.prepare(`
        UPDATE financial_months
        SET
          expected_revenue=?,
          received_revenue=?,
          expenses=?,
          total_units=?,
          delinquent_units=?
        WHERE id=?
      `).run(
        +b.expected_revenue || 0,
        +b.received_revenue || 0,
        +b.expenses || 0,
        +b.total_units || 0,
        +b.delinquent_units || 0,
        ex.id
      );

    } else {

      db.prepare(`
        INSERT INTO financial_months
        VALUES (?,?,?,?,?,?,?,?,?)
      `).run(
        id(),
        req.params.condo,
        b.reference_month,
        +b.expected_revenue || 0,
        +b.received_revenue || 0,
        +b.expenses || 0,
        +b.total_units || 0,
        +b.delinquent_units || 0,
        now()
      );
    }

    res.json({
      ok: true
    });
  }
);

/* AUDITORIA */

app.get(
  '/api/audit/:condo',
  auth,
  (req, res) => {

    const m =
      member(
        req.user.id,
        req.params.condo
      );

    if (!canManage(m)) {
      return res
        .status(403)
        .json({
          error:
            'Sem permissão'
        });
    }

    const rows =
      db.prepare(`
        SELECT
          a.*,
          u.name user_name
        FROM audit_logs a
        LEFT JOIN users u
        ON u.id=a.user_id
        WHERE a.condominium_id=?
        ORDER BY a.created_at DESC
        LIMIT 200
      `).all(
        req.params.condo
      );

    res.json(rows);
  }
);

/*
  FALLBACK DA INTERFACE
  index.html está na raiz
*/

app.use(
  (req, res) => {

    res.sendFile(
      path.join(
        __dirname,
        'index.html'
      )
    );
  }
);

app.listen(
  PORT,
  () => {

    console.log(
      `PREDIAL RAJA V2 em http://localhost:${PORT}`
    );
  }
);
